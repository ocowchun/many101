>Manage local application configuration files using templates and data from etcd or consul


### 設定key
```
curl -X PUT -d 'db.example.com' http://localhost:8500/v1/kv/myapp/database/url
curl -X PUT -d 'rob' http://localhost:8500/v1/kv/myapp/database/user
```

###建立confd的資料夾
```
sudo mkdir -p /etc/confd/{conf.d,templates}
```

###建立template resource config(template檔案的設定，包括會有哪些變數，template的檔案在哪裡,要輸出到哪裡)
/etc/confd/conf.d/myconfig.toml
```
[template]
src = "myconfig.conf.tmpl"
dest = "/tmp/myconfig.conf"
keys = [
    "/myapp/database/url",
    "/myapp/database/user",
]
```

###建立template
```
[myconfig]
database_url = {{getv "/myapp/database/url"}}
database_user = {{getv "/myapp/database/user"}}
```

###產生config
```
confd -onetime -backend consul -node 127.0.0.1:8500
```

###監聽consul上資料的變化,定期更新config
```
confd -interval 5 -backend consul -node 127.0.0.1:8500
```