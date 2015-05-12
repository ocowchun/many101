### 1. add config to your `.kitchen.yml`

```yml
---
driver:
  name: ssh
  hostname: server-ip
  port: 22
  username: user-name
  ssh_key: /Users/user-name/xxx.pem
```

###2. run `chef exec kitchen create`

###3. run `chef exec kitchen converge`
