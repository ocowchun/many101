https://www.elastic.co/guide/en/elasticsearch/guide/master/custom-analyzers.html

https://www.elastic.co/guide/en/elasticsearch/reference/2.0/indices-update-settings.html#update-settings-analysis

##修改index的設定
`put your_index/_settings`

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
