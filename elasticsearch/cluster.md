##how to build cluster


##study how to use elb with elasticsearch

https://www.nginx.com/blog/nginx-elasticsearch-better-together/
http://stackoverflow.com/questions/24751025/is-using-a-load-balancer-with-elasticsearch-unnecessary

http://stackoverflow.com/questions/24917169/how-do-i-set-up-elasticsearch-nodes-on-ec2-so-they-have-consistent-dns-entries


aws elasticsearch service 目前不提供custom plugins


##unicast
每個instance的`discovery.zen.ping.unicast.hosts`要放入其他instance的ip，就會自動變成一個cluster,這樣當一台instance掛掉的時候，其他機器會自己去推派master