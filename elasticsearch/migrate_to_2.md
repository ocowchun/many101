##migration plugin
>偵測migrate 到2.0會有哪些問題的plugin

https://github.com/elastic/elasticsearch-migration

主要的影響應該是Query DSL的變動
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_query_dsl_changes.html

###Type name prefix removed
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_mapping_changes.html#_type_name_prefix_removed

###Field names may not contain dots
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_mapping_changes.html#_field_names_may_not_contain_dots

###Query DSL changes
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_query_dsl_changes.html

###search_type=count deprecated!!!
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_search_changes.html

###All plugins are now required to have a plugin-descriptor.properties file. 
https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking_20_plugin_and_packaging_changes.html#_support_for_official_plugins