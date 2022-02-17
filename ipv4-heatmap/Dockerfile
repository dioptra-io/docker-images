FROM ubuntu:20.04 AS builder
ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /app

RUN apt-get update \
    && apt-get install --no-install-recommends --yes \
        build-essential \
        gifsicle  \
        libgd-dev \
    && rm -rf /var/lib/apt/lists/*

COPY ipv4-heatmap .
RUN make

ENTRYPOINT ["/app/ipv4-heatmap"]
