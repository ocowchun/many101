##how to build cluster


##study how to use elb with elasticsearch

https://www.nginx.com/blog/nginx-elasticsearch-better-together/
http://stackoverflow.com/questions/24751025/is-using-a-load-balancer-with-elasticsearch-unnecessary

http://stackoverflow.com/questions/24917169/how-do-i-set-up-elasticsearch-nodes-on-ec2-so-they-have-consistent-dns-entries


aws elasticsearch service 目前不提供custom plugins


##unicast
每個instance的`discovery.zen.ping.unicast.hosts`要放入其他instance的ip，就會自動變成一個cluster,這樣當一台instance掛掉的時候，其他機器會自己去推派master

## Life inside a Cluster
https://www.elastic.co/guide/en/elasticsearch/guide/current/_an_empty_cluster.html
Elasticsearch 是 single master 的架構
我們可以直接對任意 cluster 裡面的任意一個 node 發出請求, 收到請求的 node 會自動將請求轉到適合的 node
When sending requests, it is good practice to **round-robin through all the nodes in the cluster**, in order to spread the load.

一個 shard 就是一個 Lucene instance, documents 會被儲存在 shards 裡面,
不過我們不會直接跟 shard 溝通, app 只跟 index 溝通, index 再去跟 shards 溝通

shards 分成 primary shards 跟 replica shards

### primary shards
每份 document 都只會儲存在一個 primary shards, 同一份 doc 不會在兩個或以上的 primary shards 上出現
每個 shards 最多可以儲存 `Integer.MAX_VALUE - 128 ` 個 documents,
也就是說一個 index 可以擁有的最大 documents 數目會取決於他有幾個 primary shards
primary shards 的數目決定後就不可以改變

### replica shards
primary shards 的副本, 備份資料來避免 primary shards 有問題時, 資料遺失的情況。同時也可以接收讀取資料的請求
replica shards 的數目可以隨著需求做調整

## optimistic-concurrency-control
https://www.elastic.co/guide/en/elasticsearch/guide/current/optimistic-concurrency-control.html
使用樂觀鎖作控制, 不過在 logging 的情境比較不會有這方面的問題。
可以透過在修改 doc 的請求加上目前 doc 的版本號碼, 來做到 concurrency control

## Routing a Document to a Shard
https://www.elastic.co/guide/en/elasticsearch/guide/current/routing-value.html

我要怎麼知道 document 會被儲存在哪一個 shards 呢?
```
shard = hash(routing) % number_of_primary_shards
```
routing 預設是 `_id` 不過可以調整。


### distribute write(關於寫入的設定,很重要!!)
https://www.elastic.co/guide/en/elasticsearch/guide/current/distrib-write.html
write request 經由 routing 公式送到對應的 primary shards, 在 primary shard 成功寫入後,
會同步發送請求到對應的 replica shards, 等到所有的 replica shards 都回應成功,
primary shards 所屬的 node 才會跟 coordinating node 說寫入成功

#### 問題
如果有個 replica shards 一直不回應就 GG 了 QQ


寫入時根據 `consistency` 的值, 決定要寫入幾個 shards, 預設是 `quorum`
也就是 `int( (primary + number_of_replicas) / 2 ) + 1`
注意 `number_of_replicas` 是 index 設定的 replica 數, 不是目前正在運作的 replica 數。
如果可以寫入的 shard 數目小於 `consistency`, 就會等到數目足夠(timeout 是 1 min)

這邊有夠另外, 當我們只有啟用單台 Elasticsearch 的時候, quorum 為 2, 正常來說應該是不能修改資料的,
不過為了讓單機也可以正常運作, quorum 的限制只有在 number_of_replicas 大於 1 的時候才會啟用。

更新 document 的時候, primary shard 成功更新時, 會將新的 document 送到 replica shards,
而不是將 update request 送過去, 也就是說如果同時更新同一個 doc,
有可能發生 replica shards 收到的更新 doc 順序不一樣,
導致 replica shards document 的內容與 primary shard 的內容不一樣!!

### distribute read
當一個讀取的請求過來的時候, 會根據 routing 規則找對應的 shards,
然後用 round robin 的方式將 request 導向包含該 shards 的 node


### why bulk use newline to seprate multi request
tldr for performance
https://www.elastic.co/guide/en/elasticsearch/guide/current/distrib-multi-doc.html