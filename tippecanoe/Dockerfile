FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install --no-install-recommends --quiet --yes \
        build-essential \
        libsqlite3-dev \
        zlib1g-dev && \
    rm --force --recursive /var/lib/apt/lists/*

WORKDIR /tippecanoe
ADD tippecanoe/ .
RUN make --jobs=4 && make install

ENTRYPOINT ["tippecanoe"]
