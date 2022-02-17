FROM golang:1.17-alpine AS build-env
RUN apk add --update zstd-static zstd-dev make gcc musl-dev
WORKDIR /go/src/app
COPY chproxy .
RUN go build -o /go/bin/app

FROM alpine
COPY --from=build-env /go/bin/app /chproxy
ENTRYPOINT ["/chproxy"]
