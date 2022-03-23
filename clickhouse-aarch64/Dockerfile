FROM ubuntu:20.04

ENV CLICKHOUSE_UID=0
ENV CLICKHOUSE_GID=0

RUN apt-get update && \
    apt-get install -y -q --no-install-recommends \
        ca-certificates curl gosu wget zstd && \
    rm -rf /var/lib/apt/lists/*

RUN curl -L https://github.com/dioptra-io/clickhouse-builds/releases/download/20211210/clickhouse.$(arch).zst | zstd -d > /usr/bin/clickhouse && \
    chmod +x /usr/bin/clickhouse && \
    ln -s /usr/bin/clickhouse /usr/bin/clickhouse-client && \
    ln -s /usr/bin/clickhouse /usr/bin/clickhouse-server

RUN mkdir -p /etc/clickhouse-server/config.d && \
    curl -o /etc/clickhouse-server/config.xml 'https://raw.githubusercontent.com/ClickHouse/ClickHouse/master/programs/server/config.xml' && \
    curl -o /etc/clickhouse-server/users.xml 'https://raw.githubusercontent.com/ClickHouse/ClickHouse/master/programs/server/users.xml' && \
    curl -o /etc/clickhouse-server/config.d/docker_related_config.xml 'https://raw.githubusercontent.com/ClickHouse/ClickHouse/master/docker/server/docker_related_config.xml'

RUN curl -o /usr/bin/entrypoint.sh https://raw.githubusercontent.com/ClickHouse/ClickHouse/master/docker/server/entrypoint.sh && \
    chmod +x /usr/bin/entrypoint.sh
RUN mkdir /docker-entrypoint-initdb.d

ENTRYPOINT ["entrypoint.sh"]
