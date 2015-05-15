##How to install kibana in Ubuntu

###1. download and extract kibana 

```sh
$ curl -O http://download.elastic.co/kibana/kibana/kibana-4.1.0-snapshot-linux-x64.tar.gz
$ tar cvf kibana-4.1.0-snapshot-linux-x64.tar.gz
```

###2. install pm2

```sh
$ sudo npm install -g pm2
```

###3. edit `config/kibana.yml` in `kibana-4.1.0-snapshot-linux-x64`

```yml
elasticsearch_url: "http://your-elasticsearch-url:9200"
```

###4. add `kibana-process.json` to kibana folder

```js
{
  "script": "./src/bin/kibana.js",
  "max_memory_restart": "100M",
  "env": {
    "NODE_ENV": "production",
    "CONFIG_PATH": "./config/kibana.yml"
  }
}
```

###5. start kibana

```
$ pm2 start kibana-process.json
```
