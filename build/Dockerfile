FROM golang:1.15.2 AS builder

RUN mkdir -p /workdir
WORKDIR /workdir
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -mod=mod -a --installsuffix cgo --ldflags="-s" -o build/_output/bin/IngressMonitorController ./cmd/manager

FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

ENV USER_UID=1001

COPY --from=builder /workdir/build/_output/bin/* /usr/local/bin/

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}

MAINTAINER "Stakater Team"