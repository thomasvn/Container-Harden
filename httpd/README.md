## Build and Run
```
docker build -t httpd-hardened .
docker run -dit --name httpd -p 8080:80 httpd-hardened
```