FROM fedora:24 as builder
ENV DEBIAN_FRONTEND=noninteractive

RUN dnf install -y \
    git \
    gcc-c++ \
    make \
    autoconf \
    automake \
    libtool \
    lua-devel \
    libpcap-devel \
    json-c-devel \
    gnutls-devel \
    lua-ldoc \
    libnetfilter_queue-devel \
    && dnf clean all \
    && rm -rf /var/cache/yum


WORKDIR /tracebox
RUN git clone https://github.com/tracebox/tracebox .
RUN ./bootstrap.sh
RUN ./configure
RUN make
RUN make install

ENTRYPOINT [ "tracebox" ]
