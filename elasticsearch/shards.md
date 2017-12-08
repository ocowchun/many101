https://www.elastic.co/guide/en/elasticsearch/guide/current/making-text-searchable.html

## segment
每個 doc 在 index 後會被送到 buffer, buffer commited 的時候會把 buffer 內的所有 docs 都打包成一個 segment
然後寫入硬碟(fsync)。

當執行一個 query 的時候, 會搜尋所有的 segments。

segment 是 immutable, 所以資料是無法變動的, 當要刪除或修改文件的時候, 其實是去將舊文件標示為 `deleted`


https://www.elastic.co/guide/en/elasticsearch/guide/current/near-real-time.html

在 doc 被打包成 segment 之前, 是無法被搜尋到的, 可是如果每次新增一個 document 就打包一個 segment
又會造成嚴重的 performance issue, Elasticsearch 的做法是將 memory buffer 的資料寫入 filesystem cache
,之後再將資料寫入硬碟, 只要資料儲存在 cache 就可以被視為一個 segment 所以可以被搜尋的到

### refresh API
在 Elasticsearch, 可以使用 refresh API 將新資料寫入新的 segment, 預設每秒鐘會執行一次,
這讓我們可以近乎即時地搜尋到新建立的文件

透過執行下面的指令, 會手動 refresh `blog` index
```
POST /blogs/_refresh
```

有些時候你並不需要及時的搜尋能力, 比方說 logging 的時候, 通常需要的是寫入速度, 這時候可以透過調整
`refresh_interval` 來改善寫入速度。

```
PUT /my_logs
{
  "settings": {
    "refresh_interval": "30s"
  }
}
```


### flush API
https://www.elastic.co/guide/en/elasticsearch/guide/current/translog.html
將 filesystem cahce 寫入 disk(fsync), 預設 30 min 執行一次

為了確保在 filesystem cache 的資料可以被保存下來, 我們會另外建立一個 translog 來記錄尚未寫入 disk 的操作
translog 預設每五秒或是任何 write request完成時,寫入 disk(fsync)

### merge and optimize API
前面提到了, 為了讓新建立的文件可以接近即時的被搜尋到, 預設會每秒產生一個 segment,
可是每個 segment 都會需要的 file handles, memory, CPU... 更重要的是搜尋時需要檢索每一個 segment,
segment 越多, 就需要檢索越多, 影響搜尋時的速度。

Elasticsearch 可以透過在 background 執行 merge segment 來舒緩這樣的問題,
merge 時會清除掉舊 segment 中被刪除的 docs, 這些被刪除的 docs 在 merge 的時候才真正地消失。

merge 預設會自動執行


merge 會使用大量的 IO, CPU, Elasticsearch 為了避免 merge 影響搜尋的 performance, 會利用 throttle 的方式
來限制 merge 可以使用的系統資源

optimize API 可以使用 `max_num_segments` 要求 shards 將 segments merge 成指定數量,
這樣的目的在於減少 segments 來提升搜尋速度

optmize 應該只用於 static index(沒有資料變動的 index), dynamic index 使用 optmize 會有很多的問題!

logging 是很適合使用 optmize 的情境, 因為過去的 index 不會有資料變動的問題,
比方說昨天的 index 不會再有任何 log 被寫入, 這種情況下使用 optimize 將 segment merge 到只剩一個
可以減少使用的系統資源, 也提高搜尋速度。

```
POST /logstash-2014-10/_optimize?max_num_segments=1
```

執行 optmize API 時, 是不會 throttle 的!, 也就是說會使用該 node 上全部的系統資源來做 merge,
這會造成 index 短時間內無法搜尋, 並且有可能造成該 node 短時間內無法回應,
記得先將 index 搬到安全的 node 再來執行 optmize
https://www.elastic.co/guide/en/elasticsearch/guide/current/indexing-performance.html#segments-and-merging


## question
如何知道現在有多少 segments?
