FROM golang:1.10.3-alpine3.7 as builder
COPY . /app
RUN \
    cd / && \
    apk update && \
    apk add --no-cache git ca-certificates make tzdata && \
    cd app && \
    go get -d -v && \
    CGO_ENABLED=0 GOOS=linux go build -v -a -installsuffix cgo -o client 

FROM alpine:3.9
COPY --from=builder /app/client /
RUN apk add --no-cache ca-certificates tzdata tini
USER nobody
EXPOSE 9087
ENV PROXYURL=http://127.0.0.1:8080
ENV FQDN=localhost
ENV LOGLEVEL=info
ENTRYPOINT ["/sbin/tini", "--"]
CMD /client --proxy-url=$PROXYURL --fqdn=$FQDN --log.level=$LOGLEVEL
