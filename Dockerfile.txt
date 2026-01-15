ARG BASE_VERSION=3.17
FROM alpine:$BASE_VERSION

LABEL NAME=rajkatkar
LABEL EmailId=rajkatkar@gmail.com
LABEL IMAGENAME=alpine:3.17

RUN apk add --no-cache nginx curl

RUN adduser -D -u 1002 -s /bin/sh raj

COPY demo.html index.html /var/www/localhost/htdocs
COPY default.conf /etc/nginx/http.d/

RUN touch /run/nginx/nginx.pid && \
    chown -R raj:raj /run/nginx/nginx.pid && \
    chown -R raj:raj /var/lib/nginx && \
    chown -R raj:raj /var/www/localhost/htdocs && \
    chown -R raj:raj /var/log/nginx

VOLUME /var/www/localhost/htdocs

EXPOSE 8090

USER raj

ENTRYPOINT ["nginx", "-g"]
CMD ["daemon off;"]

HEALTHCHECK CMD curl -f http://localhost:8090 || exit 1
