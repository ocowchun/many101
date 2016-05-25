#synonym
>處理同義字

https://www.elastic.co/guide/en/elasticsearch/reference/2.0/analysis-synonym-tokenfilter.html


```json
{
    "settings": {
        "analysis": {

            "filter": {
                "sudo_synonym": {
                    "type": "synonym",
                    "synonyms_path": "analysis/synonym.txt"
                }
            },
            "analyzer": {
                "my_analyzer2": {
                    "type": "custom",
                    "char_filter": ["html_strip"],
                    "tokenizer": "standard",
                    "filter": ["lowercase", "sudo_synonym"]
                }
            }
        }
    }
}
```
`synonyms_path` 是`config`的相對路徑，例如`analysis/synonym.txt`會對應到 `$elastic_search_conf_path/analysis/synonym.txt`



```
curl -XPOST 'localhost:9200/myindex/_close'

curl -XPUT 'localhost:9200/myindex/_settings' -d '{
    "settings": {
        "analysis": {

            "filter": {
                "sudo_synonym": {
                    "type": "synonym",
                    "synonyms_path": "analysis/synonym.txt"
                }
            },
            "analyzer": {
                "my_analyzer2": {
                    "type": "custom",
                    "char_filter": ["html_strip"],
                    "tokenizer": "standard",
                    "filter": ["lowercase", "sudo_synonym"]
                }
            }
        }
    }
}'

curl -XPOST 'localhost:9200/myindex/_open'
```