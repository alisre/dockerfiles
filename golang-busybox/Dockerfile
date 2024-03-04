FROM golang:1.22.0-alpine3.19 AS build
ENV GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct 

COPY . /app
WORKDIR /app

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app main.go

FROM busybox:1.36-glibc
MAINTAINER autosre <idouying@gmail.com>

COPY ./bash ./curl  /bin/
COPY --from=build /app/app /app/
COPY --from=build /etc/ssl/certs /etc/ssl/certs
COPY ./.bashrc /app/
RUN addgroup -g 1000 appuser && \
    adduser -u 1000 -G appuser -S -s /bin/bash -h /app appuser && \
    chown -R appuser:appuser /app
WORKDIR /app   
USER appuser
CMD ["/bin/app"]
