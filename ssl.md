https://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/ssl-server-cert.html
https://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/US_SettingUpLoadBalancerHTTPS.html

##Create a Server Certificate
>To create a server certificate using OpenSSL

```bash
openssl genrsa -out my-private-key.pem 2048
openssl req -sha256 -new -key my-private-key.pem -out csr.pem
```





```bash
openssl   rsa  -in your-private-key-filename  -outform PEM 
openssl x509 -inform PEM -in your-public-certificate-filename
```

##什麼是Certificate chain ?



##add ssl to ELB 

###1. generate `private key` and `CSR`
```sh
$ openssl genrsa -out my-private-key-file.pem 2048
openssl req -sha256 -new -key my-private-key-file.pem -out csr.pem
```
###2. Submit the `CSR` to a Certificate Authority

###3. receive Public Certificate from SSL

###4. add `Private Key` and `Public Certificate` to elb panel

{MD5}.yourdomain.com
CName {SHA1}.comodoca.com


##make ssl grade to A
https://www.linode.com/docs/security/security-patches/disabling-sslv3-for-poodle

https://certsimple.com/blog/are-ev-ssl-certificates-worth-it

https://weakdh.org/

https://weakdh.org/sysadmin.html