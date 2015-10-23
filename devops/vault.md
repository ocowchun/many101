#Vault
>A tool for managing secrets.

[install](https://vaultproject.io/intro/getting-started/install.html)


##start the dvault Server
###development
```
$ vault server -dev
$ export VAULT_ADDR='http://127.0.0.1:8200' 
$ vault status 
```

###write secret
```
$ vault write secret/hello value=world
```

###read secret
```
$ vault read secret/hello
$ vault read -format=json secret/hello #output data in JSON format
```

###delete secret
```
$ vault delete secret/hello
```
