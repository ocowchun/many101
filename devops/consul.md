#consul
>使用gossip protocol 來檢查機器的狀況。
https://www.consul.io/

##common commands

###run a single Consul agent in server mode(單機測試用!)
```sh
consul agent -server -bootstrap-expect 1 -data-dir /tmp/consul
```

###see the members of the Consul cluster
```sh
consul members
```

###定義服務
https://consul.io/intro/getting-started/services.html


###如何串接node
https://consul.io/intro/getting-started/join.html
```sh
$ consul join 172.20.20.11
```


##key value demo
###1. set key value in node1
`$ curl -X PUT -d 'test' http://localhost:8500/v1/kv/web/key1`

###2. get key value use consul cli from node2
`$ consul watch -type=key -key=web/key1`


##[consul-template](https://github.com/hashicorp/consul-template)
使用`consul-template`來動態調整config

```
{{range ls "service/rails_app_name/env"}}
{{.Key}}: {{.Value}}
{{end}}
```