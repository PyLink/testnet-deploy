# Docker image for Nefarious IRCu (git)
FROM alpine:latest AS builder

RUN apk add --no-cache \
        git \
        autoconf \
        build-base \
        flex \
        bison \
        linux-headers \
        openssl \
        openssl-dev \
    && adduser -D ircd

USER ircd
WORKDIR /home/ircd
# Creating /tmp/ssltempdata skips makepem, the built-in TLS cert generator
# For PyLink-testnet we already use Ansible to generate certs and Docker to bind-mount them
RUN git clone https://github.com/evilnet/nefarious2/ && \
    cd nefarious2 && \
    touch /tmp/ssltempdata && \
    ./configure \
        --prefix=/home/ircd/ircd \
        --with-maxcon=1024 && \
    make install -j$(nproc)

FROM alpine:latest

RUN adduser -D ircd && apk add --no-cache openssl
COPY --from=builder --chown=ircd /home/ircd/ircd /home/ircd/ircd

USER ircd
WORKDIR /home/ircd/ircd
CMD ["/home/ircd/ircd/bin/ircd", "-n"]
