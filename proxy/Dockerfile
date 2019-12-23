FROM golang:1.10.3-alpine3.7 as builder
COPY . /app
RUN \
    cd / && \
    apk update && \
    apk add --no-cache git ca-certificates make tzdata && \
    cd app && \
    go get -d -v && \
    CGO_ENABLED=0 GOOS=linux go build -v -a -installsuffix cgo -o proxy 

FROM alpine:3.9
COPY --from=builder /app/proxy /
RUN apk add --no-cache ca-certificates tzdata tini
USER nobody
EXPOSE 9087
ENV LISTENADDRESS=":8080"
ENV LOGLEVEL=info
ENTRYPOINT ["/sbin/tini", "--"]
CMD /proxy --web.listen-address=$LISTENADDRESS --log.level=$LOGLEVEL
