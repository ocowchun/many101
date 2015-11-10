##search
https://github.com/elastic/elasticsearch-rails/blob/master/elasticsearch-model/README.md#the-elasticsearch-dsl

###simple search
```
@articles = Article.search(params[:q]).page(params[:page]).records
```

###search with specify column
```rb
query={query:{ match:  { title: "foo" } }}
@articles = Article.search(query).records
```

###分頁
`elasticsearch-model`有支援`kaminari`所以可以直接使用，不過要注意的是在`Gemfile`裡面，要把`kaminari`放在`elasticsearch-model`前面(The pagination gems must be added before the Elasticsearch gems in your Gemfile, or loaded first in your application.)


###[序列化](https://github.com/elastic/elasticsearch-rails/tree/master/elasticsearch-model#model-serialization)

###what is boost? 
A boost parameter can be specified to give this term query a higher relevance score than another query, for instance:
