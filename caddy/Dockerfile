FROM alpine:3
LABEL maintainer "Pavel Pavlenko <region23@gmail.com>"

ARG EMAIL
ARG CADDY_PLUGINS

RUN apk add --no-cache openssh-client git tar curl hugo

# Устанавливаем пакет с таймзонами для корректной работы time.LoadLocation в Go
RUN apk update && apk upgrade && apk add -U tzdata && apk add ca-certificates

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN mkdir -p /root/.caddy/

RUN curl --silent --show-error --fail --location \
  --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
  "https://caddyserver.com/download/linux/amd64?license=personal&telemetry=off&plugins=${CADDY_PLUGINS}" \
  | tar --no-same-owner -C /usr/bin/ -xz caddy \
  && chmod 0755 /usr/bin/caddy \
  && /usr/bin/caddy -version

EXPOSE 80 443 2015
VOLUME /root/.caddy

COPY Caddyfile /etc/Caddyfile

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile", "--log", "stdout", "--agree=true", "--email=${EMAIL}"]
