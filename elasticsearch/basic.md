##elastic 類比於RMDB
Index=>Database
Type=>Table
Document=>row
Field=>column

##建立index
`$ curl -XPUT http://localhost:9200/index`

##建立Type
```
curl -XPOST http://localhost:9200/index/fulltext/_mapping -d'
{
    "fulltext": {
             "_all": {
            "indexAnalyzer": "ik",
            "searchAnalyzer": "ik",
            "term_vector": "no",
            "store": "false"
        },
        "properties": {
            "content": {
                "type": "string",
                "store": "no",
                "term_vector": "with_positions_offsets",
                "indexAnalyzer": "ik",
                "searchAnalyzer": "ik",
                "include_in_all": "true",
                "boost": 8
            }
        }
    }
}'
```

##search
###URI Search
`http://localhost:9200/twitter/tweet/_search?q=user:kimchy`


##[檢視目前的index](https://www.elastic.co/guide/en/elasticsearch/reference/current/_create_an_index.html)
`http://192.168.33.33:9200/_cat/indices?v`

health為yellow表示replica沒有設置好(最常見的情況是你只有設置一個節點，所以沒有備份的節點)

##[Maping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)
>設定type的schema

###檢視特定index的mapping
`http://192.168.33.34:9200/index_name/_mappings/`

[設定mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html)

`/_nodes`
看節點的設置情況


 ~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e6%97%b6%e9%97%b4\&tokenizer\=s2t_keep_both_convert
{"tokens":[{"token":"北京時間,北京时间","start_offset":0,"end_offset":4,"type":"word","position":1}]}%

~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e5%9c%8b%e9%9a%9b%e9%9b%bb%e8%a6%96%e8%87%ba\&tokenizer\=t2s_keep_both_convert
{"tokens":[{"token":"北京国际电视台,北京國際電視臺","start_offset":0,"end_offset":7,"type":"word","position":1}]}%

~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e5%9c%8b%e9%9a%9b%e9%9b%bb%e8%a6%96%e8%87%ba\&tokenizer\=t2s_convert
{"tokens":[{"token":"北京国际电视台","start_offset":0,"end_offset":7,"type":"word","position":1}]}%

~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e5%9c%8b%e9%9a%9b%e9%9b%bb%e8%a6%96%e8%87%ba\&analyzer\=t2s_convert
{"tokens":[{"token":"北京国际电视台","start_offset":0,"end_offset":7,"type":"word","position":1}]}%

~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e5%9c%8b%e9%9a%9b%e9%9b%bb%e8%a6%96%e8%87%ba\&filters\=t2s_convert
{"tokens":[{"token":"北京國際電視臺","start_offset":0,"end_offset":7,"type":"word","position":1}]}%

~  curl -XGET http://localhost:9200/index/_analyze\?text\=%e5%8c%97%e4%ba%ac%e5%9c%8b%e9%9a%9b%e9%9b%bb%e8%a6%96%e8%87%ba\&tokenizer\=keyword\&filters\=t2s_convert
{"tokens":[{"token":"北京国际电视台","start_offset":0,"end_offset":7,"type":"word","position":1}]}%

##ELASTICSEARCH ANALYZER 的內部機制
http://mednoter.com/all-about-analyzer-part-one.html
https://www.elastic.co/blog/found-text-analysis-part-1