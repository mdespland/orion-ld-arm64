ARG PG_VERSION_TAG
FROM timescale/timescaledb:2.3.1-${PG_VERSION_TAG}

LABEL maintainer="Timescale https://www.timescale.com"
ARG POSTGIS_VERSION
ENV POSTGIS_VERSION ${POSTGIS_VERSION:-2.5.3}

RUN set -ex \
    && apk add --no-cache --virtual .fetch-deps \
                ca-certificates \
                openssl \
                tar \
    # add libcrypto from (edge:main) for gdal-2.3.0
    #&& apk add --no-cache --virtual .crypto-rundeps \
    #            --repository http://dl-cdn.alpinelinux.org/alpine/edge/main \
    #            libressl2.7-libcrypto \
    #            libcrypto1.1 \
    #            poppler \
    #            llvm9-dev \
    && apk add --no-cache --virtual .build-deps \
        autoconf \
        automake \
        file \
        json-c-dev \
        libtool \
        libxml2-dev \
        make \
        perl \
        llvm \
        clang \
        clang-dev \
        g++ \
        gdal-dev \
        geos-dev \
        proj-dev \
        protobuf-c-dev \
    && cd /tmp \
    && wget https://github.com/postgis/postgis/archive/${POSTGIS_VERSION}.tar.gz -O - | tar -xz \
    && chown root:root -R postgis-${POSTGIS_VERSION} \
    && cd /tmp/postgis-${POSTGIS_VERSION}
WORKDIR /tmp/postgis-${POSTGIS_VERSION}
RUN ./autogen.sh
RUN touch ./build-aux/config.rpath
RUN ./configure \
    && echo "PERL = /usr/bin/perl" >> extensions/postgis/Makefile \
    && echo "PERL = /usr/bin/perl" >> extensions/postgis_topology/Makefile
RUN make -s
RUN make -s install 
RUN apk add --no-cache --virtual .postgis-rundeps \
        json-c \
        geos \
        gdal \
        proj \
        protobuf-c \
        libstdc++ 
WORKDIR /
RUN rm -rf /tmp/postgis-${POSTGIS_VERSION} 
RUN apk del .fetch-deps .build-deps