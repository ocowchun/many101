(How To Create Temporary and Permanent Redirects with Apache and Nginx)[https://www.digitalocean.com/community/tutorials/how-to-create-temporary-and-permanent-redirects-with-apache-and-nginx]

```
server {
  listen 80;
  server_name domain1.com;
  return 301 $scheme://domain2.com$request_uri;
}
```
