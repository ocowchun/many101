##bool search
[Bool Query](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/query-dsl-bool-query.html)
[Minimum Should Match](https://www.elastic.co/guide/en/elasticsearch/reference/1.7/query-dsl-minimum-should-match.html)

https://www.elastic.co/guide/en/elasticsearch/guide/current/search.html
While many searches will just work out of the box, to use Elasticsearch to its full potential, you need to understand three subjects:

### Mapping
How the data in each field is interpreted
### Analysis
How full text is processed to make it searchable
### Query DSL
The flexible, powerful query language used by Elasticsearch


你可以在 search 指定 timeout,
```
GET /_search?timeout=10ms
```

這會讓 node 將指定的時間內搜集到的 result 返回, 要注意的是就算請求返回了, 那些尚未返回 node 還在執行的 query 還是會繼續執行, 直到他們完成工作，也就是說 timeout 並不會讓 cluster 的工作量減少, 只是為了滿足 app 對 response time 的要求。

### paging
https://www.elastic.co/guide/en/elasticsearch/guide/current/pagination.html
```
GET /_search?size=5&from=5
```
`size`: 指定返回的文件數(default: 10)
`from`: 從第幾個開始返回

注意!! 越後面的 page 會有越嚴重的 performance issue

paging 的運作方式如下
假設我的 index 有五個 primary shards, size = 10, from = 0
會從五個 shards 取回各自排名前 10 的 doc, 然後送到 coordinating node 將 50 個 doc 彙整做排序, 取出分數最高的前十

如果 size = 10, from = 10000
就會從五個 shards 取回各自排名前 10000 的 doc, 然後有總共 50000 個 doc 被送到 coordinating node 接著做排序!


### _all
index 時, 預設會產生一個 `_all` 欄位將所有個 filed 結合再一起, 例如:

```js
{
    "tweet":    "However did I manage before Elasticsearch?",
    "date":     "2014-09-14",
    "name":     "Mary Jones",
    "user_id":  1
}
```

會產生 `_all` 值為

```
"However did I manage before Elasticsearch? 2014-09-14 Mary Jones 1"
```

如果用不到 `_all` 的話可以取消 https://www.elastic.co/guide/en/elasticsearch/guide/current/root-object.html#all-field


## Exact Values Versus Full Text
https://www.elastic.co/guide/en/elasticsearch/guide/current/_exact_values_versus_full_text.html

full text value 會執行 analyze 然後做分詞, exact value 不會

https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html

