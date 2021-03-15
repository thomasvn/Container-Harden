## Build and Run
```
make
```

## Using an Ironbank Image
SETUP
```
# authenticate with IronBank
docker login <registry-name>

# generate localhost ssl certificates (macOS)
# https://stackoverflow.com/questions/8169999/how-can-i-create-a-self-signed-cert-for-localhost
openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt
open localhost.crt

# Move SSL Certs to correct directory within the container
# The below commands are placed inside the Dockerfile
COPY --chown=root:root ./certs/localhost.crt /etc/pki/tls/certs/
COPY --chown=root:root ./certs/localhost.key /etc/pki/tls/private/
```

DEBUGGING
```
# Override the default entrypoint to get a shell in the container
docker run -dit -p 8443:8443 --entrypoint /bin/bash <image-name>
```