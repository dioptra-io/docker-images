# Builder
FROM ubuntu:20.04 as builder
ENV DEBIAN_FRONTEND=noninteractive

ARG bazel_version=4.2.1

RUN apt-get update && \
    apt-get install --no-install-recommends --quiet --yes \
        build-essential \
        ca-certificates \
        curl \
        git \
        libgflags-dev \
        libgoogle-glog-dev && \
    rm --force --recursive /var/lib/apt/lists/*

RUN curl --location --output /usr/bin/bazel https://github.com/bazelbuild/bazel/releases/download/${bazel_version}/bazel-${bazel_version}-linux-$(arch | sed -e "s/aarch64/arm64/") && \
    chmod +x /usr/bin/bazel

WORKDIR /flashroute
ADD FlashRoute/ .
RUN bazel build --cxxopt="--std=c++14" --compilation_mode=opt flashroute

# Main
FROM ubuntu:20.04
COPY --from=builder /flashroute/bazel-bin/flashroute/flashroute /usr/bin/flashroute
ENTRYPOINT ["flashroute"]
