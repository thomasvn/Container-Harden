## Build and Run
```
make
```

## Using an Ironbank Image
SETUP
```sh
# authenticate with IronBank
docker login <registry-name>

# generate localhost ssl certificates (macOS)
# https://stackoverflow.com/questions/8169999/how-can-i-create-a-self-signed-cert-for-localhost
openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt
open localhost.crt
```

ADD TO DOCKERFILE
```Dockerfile
# Certs for TLS
COPY --chown=httpd:httpd ./certs/localhost.crt /etc/pki/tls/certs/
COPY --chown=httpd:httpd ./certs/localhost.key /etc/pki/tls/private/

# These are files which are in the httpd root but have not been assigned the
# proper privileges because they are symlinked into the httpd root
USER root
RUN chown -R httpd:httpd /run/httpd && \
    chown -R httpd:httpd /var/log/httpd && \
    chown -R httpd:httpd /usr/lib64/httpd/modules && \
    chown -R httpd:httpd /var/lib/httpd
USER httpd
```

DEBUGGING
```sh
# Override the default entrypoint to get a shell in the container
docker run -dit -p 8443:8443 --entrypoint /bin/bash <image-name>
```