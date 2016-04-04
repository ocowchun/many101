##score
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-function-score-query.html


###script_score
>預設的計算方式是原始分數*`script的分數`，如果你想要用`script的分數`取代原始的分數的話，可以透過`"boost_mode": "replace"`的方式達成。

###field_value_factor
>可以將doc的欄位帶入計算分數，跟script_score有點像，不過簡化了一點。

