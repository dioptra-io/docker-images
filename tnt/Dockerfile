FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install --no-install-recommends --quiet --yes \
        automake \
        build-essential \
        libtool && \
    rm --force --recursive /var/lib/apt/lists/*

WORKDIR /scamper
ADD scamper/ .
RUN autoreconf -i && ./configure && make && make install && ldconfig
