FROM andrewnk/turtlecoin:base as base

FROM alpine

RUN addgroup -S turtlecoin && adduser -S turtlecoin -G turtlecoin -h /home/turtlecoin && \
    apk add --no-cache 'su-exec>=0.2'

# Remote host to connect to
ARG REMOTE_DAEMON_HOST
ENV REMOTE_DAEMON_HOST=${REMOTE_DAEMON_HOST}

# Remote port to connect to
ARG REMOTE_DAEMON_PORT
ENV REMOTE_DAEMON_PORT=${REMOTE_DAEMON_PORT}

# Web username
ARG WEB_USERNAME
ENV WEB_USERNAME=${WEB_USERNAME}

# Web username
ARG WEB_PASSWORD
ENV WEB_PASSWORD=${WEB_PASSWORD}

# add library required to run binary, ttyd, and tmux
RUN apk add --no-cache libucontext-dev ttyd tmux

#uncomment when the ttyd ssl bug is fixed
#RUN apk add --no-cache openssl && \
#    mkdir -p /ttyd/certs
#
#WORKDIR /ttyd/certs
#
# create a self signed ssl cert for secure ttyd
#RUN openssl genrsa -out ca.key 4096 && \
#    openssl req -new -x509 -days 365 -key ca.key -out ca.crt -subj "/C=TC/ST=TurtleCoin/L=TurtleCoin/O=TurtleCoin/OU=TurtleCoin/CN=localhost" && \
#    openssl req -newkey rsa:2048 -nodes -keyout server.key -out server.csr -subj "/C=TC/ST=TurtleCoin/L=TurtleCoin/O=TurtleCoin/OU=TurtleCoin/CN=localhost" && \
#    openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt && \
#    openssl req -newkey rsa:2048 -nodes -keyout client.key -out client.csr -subj "/C=TC/ST=TurtleCoin/L=TurtleCoin/O=TurtleCoin/OU=TurtleCoin/CN=localhost" && \
#    openssl x509 -req -days 365 -in client.csr -CA ca.crt -CAkey ca.key -set_serial 02 -out client.crt && \
#    chown turtlecoin *

#copy binary from base
COPY --from=base /zedwallet /usr/local/bin

VOLUME /home/turtlecoin
WORKDIR /home/turtlecoin

COPY ./docker-entrypoint.sh /usr/local/bin/

# fix ownership
RUN chown -R turtlecoin:turtlecoin . && \
    chmod 777 /usr/local/bin/docker-entrypoint.sh && \
    ln -s /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["docker-entrypoint.sh"]
