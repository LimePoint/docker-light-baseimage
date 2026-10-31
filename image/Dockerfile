FROM golang:1.27-alpine AS cfssl

ARG CFSSL_VERSION=1.7.1
RUN CGO_ENABLED=0 go install -trimpath -ldflags=-s \
    github.com/cloudflare/cfssl/cmd/cfssl@v${CFSSL_VERSION} \
    github.com/cloudflare/cfssl/cmd/cfssljson@v${CFSSL_VERSION}

FROM debian:trixie-slim

COPY . /container
RUN /container/build.sh
COPY --from=cfssl --chmod=700 /go/bin/cfssl /go/bin/cfssljson /usr/sbin/

ENV LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LC_ALL="en_US.UTF-8"

ENTRYPOINT ["/container/tool/run"]
