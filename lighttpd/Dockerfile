FROM alpine:3.9

# Install packages
RUN apk update && \
    apk add --no-cache \
    lighttpd \
    openssl \
    curl && \
    rm -rf /var/cache/apk/*

# generate self-signed cert
RUN mkdir /etc/lighttpd/ssl/ && \
	openssl req -x509 -newkey rsa:4096 -keyout /tmp/key.pem -out /tmp/cert.pem -days 365 -subj '/CN=localhost' -nodes -sha256 && \
	cat /tmp/key.pem /tmp/cert.pem > /etc/lighttpd/ssl/localhost.pem && \
	rm /tmp/key.pem /tmp/cert.pem && \
	chmod 400 /etc/lighttpd/ssl/localhost.pem

# copy config
# COPY etc/lighttpd/*.conf /etc/lighttpd/

# copy files
#RUN mkdir /var/www/html
COPY htdocs/* /var/www/localhost/

RUN mkdir -p /var/cache/lighttpd/uploads

# HEALTHCHECK --interval=1m --timeout=1s \
#   CMD curl -f http://localhost/ || exit 1

# Expose http(s) ports
EXPOSE 80 443

ENTRYPOINT ["/usr/sbin/lighttpd", "-D", "-f", "/etc/lighttpd/lighttpd.conf"]