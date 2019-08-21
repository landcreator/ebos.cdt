FROM boscore/builder as builder
ARG branch=master
ARG bosbranch=master

ENV EOSIO_ROOT=/opt/eosio
ENV OPENSSL_ROOT_DIR /usr/include/openssl
ENV PATH /opt/eosio/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV INSTALL_PREFIX=/usr/local/eosio

RUN mkdir -p "${INSTALL_PREFIX}"

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install openssl ca-certificates \
            curl wget git cmake python3 g++ vim net-tools lsof wget curl supervisor libpthread-stubs0-dev

RUN git clone -b $bosbranch https://github.com/boscore/bos.git --recursive \
    && cd bos && echo "$bosbranch:$(git rev-parse HEAD)" > /etc/eosio-version \
    && cmake -H. -B"${INSTALL_PREFIX}" -GNinja -DCMAKE_BUILD_TYPE=Release -DWASM_ROOT=/opt/wasm -DCMAKE_CXX_COMPILER=clang++ \
       -DCMAKE_C_COMPILER=clang -DCMAKE_INSTALL_PREFIX="${INSTALL_PREFIX}" -DBUILD_MONGO_DB_PLUGIN=true -DCORE_SYMBOL_NAME=$symbol \
       -DOPENSSL_ROOT_DIR="${OPENSSL_ROOT_DIR}" -DCMAKE_CXX_STANDARD_LIBRARIES="-lpthread" \
    && cmake --build "${INSTALL_PREFIX}" --target install


RUN cd / && git clone -b $branch https://github.com/boscore/bos.cdt.git --recursive \
    && cd /bos.cdt/ \
    && ./build.sh \
    && ./install.sh \
    && cd .. \
    && rm -rf ./bos.cdt

RUN rm -rf /var/lib/apt/lists/*

USER root