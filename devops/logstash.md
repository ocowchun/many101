logstash_dateformat

By default, the records inserted into index logstash-YYMMDD. This option allows to insert into specified index like mylogs-YYYYMM for a monthly index.

logstash_dateformat %Y.%m. # defaults to "%Y.%m.%d"
