FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive

ARG scamper_version=scamper-cvs-20211212d

RUN apt-get update && \
    apt-get install --yes --quiet \
      build-essential \
      ca-certificates \
      gcc \
      g++ \
      git \
      wget && \
    apt-get clean && \
    rm --force --recursive /var/lib/apt/lists/*

# download and extract source code
RUN mkdir -p scamper-src && \
    cd scamper-src && \
    wget https://www.caida.org/catalog/software/scamper/code/${scamper_version}.tar.gz && \
    tar xzf ${scamper_version}.tar.gz && cd ${scamper_version}

# create build dir
WORKDIR /scamper-src/${scamper_version}/

# build scamper
RUN ./configure --enable-debug
RUN make -j16
RUN make install
