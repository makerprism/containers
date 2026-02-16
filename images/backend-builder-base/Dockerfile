FROM alpine:3.20

ARG DUNE_VERSION=3.21.0
ARG POSTGRES_VERSION=16.3

RUN apk -U upgrade && apk add --no-cache \
    build-base \
    gmp-dev \
    libev-dev \
    libffi-dev \
    openssl-dev \
    openssl-libs-static \
    postgresql-dev \
    curl \
    git \
    bash \
    m4 \
    musl-dev \
    zlib-static \
    zlib-dev \
    perl \
    readline-dev \
    readline-static

RUN curl -fsSL https://github.com/ocaml-dune/dune-bin-install/releases/download/v3/install.sh \
    | sh -s -- "${DUNE_VERSION}" --install-root /usr --no-update-shell-config

RUN curl -fsSL "https://ftp.postgresql.org/pub/source/v${POSTGRES_VERSION}/postgresql-${POSTGRES_VERSION}.tar.gz" | tar xzf - && \
    cd "postgresql-${POSTGRES_VERSION}" && \
    ./configure --prefix=/usr/local/pgsql --without-readline --without-zlib --without-icu && \
    cd src/interfaces/libpq && make && make install && \
    cd ../../common && make && ar rcs libpgcommon.a *.o && \
    cd ../port && make && ar rcs libpgport.a *.o && \
    mkdir -p /usr/local/pgsql/lib && \
    cp libpgport.a /usr/local/pgsql/lib/ && \
    cp ../common/libpgcommon.a /usr/local/pgsql/lib/ && \
    cd ../../../ && rm -rf "postgresql-${POSTGRES_VERSION}"

WORKDIR /app
