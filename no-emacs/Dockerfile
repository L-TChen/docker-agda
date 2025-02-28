FROM alpine:latest AS base

RUN apk upgrade --no-cache &&\
    apk add --no-cache \
        cabal \
        g++ \
        gcc \
        ncurses-dev \
        ncurses-static \
        musl-dev \
        zlib-static \
        zlib-dev

FROM base AS build

ARG PKG_VER=2.7.0.1
ARG PKG_BUILD_FLAGS="--enable-split-sections -foptimise-heavily -fdebug"

RUN wget -qO- https://hackage.haskell.org/package/Agda-${PKG_VER}/Agda-${PKG_VER}.tar.gz | tar xz

WORKDIR Agda-${PKG_VER}

RUN cabal update
RUN cabal build ${PKG_BUILD_FLAGS} --dependencies-only
RUN cabal build ${PKG_BUILD_FLAGS} --enable-executable-static

RUN mkdir -p /opt/agda/bin &&\
    find dist-newstyle/build \( -name 'agda' -o -name 'agda-mode' \) -type f -exec cp {} /opt/agda/bin \; &&\
    strip /opt/agda/bin/* &&\
    cp -a src/data/ /opt/agda/

FROM alpine:latest AS final

ARG STDLIB_VER=2.2

COPY --from=build /opt/ /opt/

ENV Agda_datadir=/opt/agda/data
ENV PATH=/opt/agda/bin:$PATH

RUN apk upgrade --no-cache &&\
    apk add     --no-cache make

# Install Agda's standard library
# TODO: make this part easier to include multiple libraries

RUN mkdir /opt/agda/lib &&\
    wget -qO- "https://github.com/agda/agda-stdlib/archive/refs/tags/v${STDLIB_VER}.tar.gz" | tar xz -C /opt/agda/lib &&\
    mkdir -p ~/.config/agda &&\
    echo "/opt/agda/lib/agda-stdlib-${STDLIB_VER}/standard-library.agda-lib" >> ~/.config/agda/libraries

ENTRYPOINT sh
