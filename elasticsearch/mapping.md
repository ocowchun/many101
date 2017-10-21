###put mapping
> The PUT mapping API allows you to add a new type to an existing index, or new fields to an existing type:
https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html

https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html

###Field datatypes
https://www.elastic.co/guide/en/elasticsearch/reference/1.4/mapping-array-type.html


https://www.elastic.co/guide/en/elasticsearch/guide/current/mapping-intro.html

### index
The index attribute controls how the string will be indexed. It can contain one of three values:

`analyzed`
First analyze the string and then index it. In other words, index this field as full text.

`not_analyzed`
Index this field, so it is searchable, but index the value exactly as specified. Do not analyze it.

`no`
Don’t index this field at all. This field will not be searchable.

The default value of index for a string field is `analyzed`.

### Complex Core Field Types
https://www.elastic.co/guide/en/elasticsearch/guide/current/complex-core-fields.html