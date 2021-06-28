FROM alpine:latest
ARG UNREAL_VERSION
RUN [ -z "$UNREAL_VERSION" ] && echo "Please set the UNREAL_VERSION build arg" && exit 1 || true

RUN apk add --no-cache \
    argon2-dev \
    build-base \
	c-ares-dev \
    libsodium-dev \
    openssl \
	openssl-dev \
	pcre2-dev \
    pkgconf \
    wget

RUN adduser -D ircd
USER ircd

RUN wget https://www.unrealircd.org/downloads/unrealircd-$UNREAL_VERSION.tar.gz -P ~/ && \
    tar xvf ~/unrealircd-$UNREAL_VERSION.tar.gz -C ~/ && \
    rm ~/unrealircd-$UNREAL_VERSION.tar.gz

RUN cd ~/unrealircd-$UNREAL_VERSION && \
    ./configure \
        --prefix=/home/ircd/unrealircd \
        --with-datadir=/home/ircd/unrealircd/data \
        --enable-dynamic-linking \
        --enable-ssl \
        --with-system-pcre2 \
        --with-system-argon2 \
        --with-system-sodium \
        --with-system-cares && \
    make -j $(nproc) && \
    make install

VOLUME /home/ircd/unrealircd/conf

CMD ["/home/ircd/unrealircd/unrealircd", "start"]
