# Container from a trusted repo. Pull with its digest. If updated container is needed, identify new digest.
FROM registry.access.redhat.com/ubi8:8.3

# Install packages as root. Verify downloaded files. Clean up unnecessary working files.
# NOTE: this example does not fully build and install httpd; demo is primarily to show sha256sum.
RUN dnf install -y wget httpd && \
    wget https://apache.claz.org//httpd/httpd-2.4.46.tar.gz && \
    wget https://downloads.apache.org/httpd/httpd-2.4.46.tar.gz.sha256 && \
    sha256sum httpd-2.4.46.tar.gz | sha256sum -c httpd-2.4.46.tar.gz.sha256 && \
    gzip -d httpd-2.4.46.tar.gz && \
    tar xvf httpd-2.4.46.tar && \
    dnf clean all && \
    rm -rf /var/cache/dnf && \
    rm -rf httpd-2.4.46 httpd-2.4.46.tar httpd-2.4.46.tar.gz httpd-2.4.46.tar.gz.sha256

# Map host's unprivileged ports to container's privileged ports (e.g. 8080 -> 80)
EXPOSE 8080/tcp

# Add a healthcheck for increased understandability when deployed by container platform
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl http://localhost/ || exit 1

# Use COPY for local files. Careful when ADDing remote files.
COPY httpd/etc.httpd.conf.httpd.conf etc/httpd/conf/httpd.conf
COPY httpd/usr.lib.tmpfiles.d.httpd.conf usr/lib/tmpfiles.d/httpd.conf

# Config changes so httpd can run as non-privileged user
RUN groupadd -r user && \
    useradd -r -g user user && \
    chown -R user /var/log/httpd && \
    chown -R user /run/httpd && \
    setcap cap_net_bind_service=+epi /usr/sbin/httpd

# Run the container as a non-privileged user
USER user

# If your app needs root privileges to run, create an entrypoint script to initialize app as root then switch to unprivileged user
ENTRYPOINT ["httpd", "-D", "FOREGROUND"]
