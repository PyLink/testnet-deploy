# Docker image for Nefarious IRCu (git)
FROM alpine:latest

# linux-headers =
RUN apk add --no-cache \
        git \
        build-base \
        flex \
        libmaxminddb-dev \
        yacc \
        bison \
        linux-headers \
        openssl \
    && adduser -D ircd

USER ircd
WORKDIR /home/ircd

# Creating /tmp/ssltempdata skips makepem, the built-in TLS cert generator
# For PyLink-testnet we already use Ansible to generate certs and Docker to bind-mount them
RUN git clone https://github.com/evilnet/nefarious2/ && \
    cd nefarious2 && \
    touch /tmp/ssltempdata && \
    ./configure && make install -j$(nproc)

CMD ["/home/ircd/bin/ircd", "-n"]
