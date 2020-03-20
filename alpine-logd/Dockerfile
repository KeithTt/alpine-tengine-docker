FROM alpine:3.7
# FROM alpine:latest

MAINTAINER "shengyongp"

ADD sources/repositories /etc/apk/repositories
ADD sources/logd /usr/local/sbin/logd
ADD sources/supervisord.conf /etc/supervisord.conf

RUN apk update && apk upgrade apk-tools && \
    apk add --no-cache supervisor tzdata && \
    mkdir /var/log/supervisor && \
    # setup timezone
    cp -a /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" >  /etc/timezone

EXPOSE 80

ENTRYPOINT ["supervisord", "-n"]
