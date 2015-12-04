#Index

##基本操作
###建立index
`$ curl -XPUT http://localhost:9200/index`

###destroy index
`$ curl -XDELETE http://localhost:9200/index`

##Index Template
>建立Index時的預設配置
https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-templates.html

curl -XPUT localhost:9200/_template/template_1

```
{
    "template" : "te*",
    "settings" : {
        "number_of_shards" : 1
    },
    "mappings" : {
        "type1" : {
            "_source" : { "enabled" : false }
        }
    }
}
```

建立一個template名字為`template1`，任何名稱符合`te*` pattern的index都會套用這個設定