https://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/ssl-server-cert.html

```bash
openssl   rsa  -in your-private-key-filename  -outform PEM 
openssl x509 -inform PEM -in your-public-certificate-filename
```

##什麼是Certificate chain ?