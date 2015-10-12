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