FROM boscore/builder as builder
ARG branch=master
ARG bosbranch=master

ENV INSTALL_PREFIX=/usr/local/eosio
ENV OPENSSL_ROOT_DIR /usr/include/openssl
ENV PATH ${INSTALL_PREFIX}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p "${INSTALL_PREFIX}"

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install openssl ca-certificates \
            curl wget git cmake python3 g++ vim net-tools lsof wget curl supervisor libpthread-stubs0-dev

RUN cd / && git clone -b $bosbranch https://github.com/boscore/bos.git --recursive \
    && cd bos && echo "$bosbranch:$(git rev-parse HEAD)" > /etc/eosio-version \
    && cmake -H. -B"/tmp/build" -GNinja -DCMAKE_BUILD_TYPE=Release -DWASM_ROOT=/opt/wasm -DCMAKE_CXX_COMPILER=clang++ \
       -DCMAKE_C_COMPILER=clang -DCMAKE_INSTALL_PREFIX="${INSTALL_PREFIX}" -DBUILD_MONGO_DB_PLUGIN=true -DCORE_SYMBOL_NAME=$symbol \
       -DOPENSSL_ROOT_DIR="${OPENSSL_ROOT_DIR}" -DCMAKE_CXX_STANDARD_LIBRARIES="-lpthread" \
    && cmake --build "/tmp/build" --target install \
    && cd .. \
    && rm -rf bos && rm -rf /tmp/build


RUN cd / && git clone -b $branch https://github.com/boscore/bos.cdt.git --recursive \
    && cd /bos.cdt \
    && ./build.sh \
    && ./install.sh \
    && cd .. \
    && rm -rf /bos.cdt

RUN rm -rf /var/lib/apt/lists/*

USER root