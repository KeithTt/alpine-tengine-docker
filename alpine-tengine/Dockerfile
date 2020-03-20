FROM alpine:3.7
# FROM alpine:latest

MAINTAINER "shengyongp"

ADD sources/repositories /etc/apk/repositories
RUN apk update && apk upgrade apk-tools && \
    apk add --no-cache alpine-sdk libressl-dev openldap-dev zlib-dev pcre-dev supervisor tzdata && \
    mkdir /usr/local/src && cd /usr/local/src && \
    git clone https://github.com/kvspb/nginx-auth-ldap.git && \
    wget http://tengine.taobao.org/download/tengine-2.2.0.tar.gz && \
    tar xvf tengine-2.2.0.tar.gz && \
    cd tengine-2.2.0/ && \
    ./configure --prefix=/usr/local/nginx --with-http_reqstat_module=shared --add-module=../nginx-auth-ldap && \
    make && make install && \
    make dso_install && \
    # nginx no-daemonize
    echo "daemon off;" >> /usr/local/nginx/conf/nginx.conf && \
    mkdir /var/log/supervisor && \
    rm -rf /usr/local/src/* && \
    # forward request and error logs to docker log collector
    ln -sf /dev/stdout /usr/local/nginx/logs/access.log && \
    ln -sf /dev/stderr /usr/local/nginx/logs/error.log && \
    # change timezone
    cp -a /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" >  /etc/timezone

ADD sources/supervisord.conf /etc/supervisord.conf

EXPOSE 80

ENTRYPOINT ["supervisord", "-n"]
