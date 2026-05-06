FROM alpine:3.20

ARG DUNE_VERSION=3.23.0
ARG POSTGRES_VERSION=16.3

RUN apk add --no-cache \
    build-base \
    ocaml \
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

# dune-bin-install is archived; build dune from the official release tarball.
RUN curl -fsSL "https://github.com/ocaml/dune/releases/download/${DUNE_VERSION}/dune-${DUNE_VERSION}.tbz" | tar xjf - && \
    cd "dune-${DUNE_VERSION}" && \
    ./configure --prefix=/usr && \
    make release && \
    make install && \
    cd .. && rm -rf "dune-${DUNE_VERSION}"

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
