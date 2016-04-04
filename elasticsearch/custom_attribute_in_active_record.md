使用elastic_search時，會有需要針對資料表產生客制屬性，方便撰寫搜尋DSL
例如地址可能是儲存 city,street 因此會合併成一個 address，或是將欄位的內容經過調整。


不過因為partial update的關係 當資料更新時，會發生客制屬性有沒有更新到的情況
可以透過對客制屬性執行`attribute_will_change`,讓elastic_search 知道要去更新這個屬性


```rb
    
    attr_accessor :categories
    before_validation :change_categories

   def categories
      [:category1,:category2].map{|c|send(c)}.join(' ').strip
    end

    # need to be call before before_save to really change categories
    # see https://github.com/elastic/elasticsearch-rails/blob/92f0148af8849dac154ee41bd1c3dd868b421b86/elasticsearch-model/lib/elasticsearch/model/indexing.rb#L313
    def change_categories
      attribute_will_change!('categories')
    end
```
