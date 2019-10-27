# this is our first build stage, it will not persist in the final image
FROM golang:1.10.0 as builder

# Clone GO Repos
RUN mkdir /go/src/github.com && mkdir /go/src/github.com/xteve-project
WORKDIR /go/src/github.com/xteve-project
RUN git clone --recurse-submodules https://github.com/xteve-project/xTeVe.git
RUN go get github.com/koron/go-ssdp \
	&& go get github.com/gorilla/websocket \
	&& go get github.com/kardianos/osext

WORKDIR $GOPATH/src/github.com/xteve-project/xTeVe

# Fix Reverse Proxy
# RUN sed -i '319,323d' xteve.go

# Build xTeVe
RUN go build xteve.go

# Second Stage Xteve
FROM ubuntu:18.04

# Updates
RUN apt-get update \
    && apt-get dist-upgrade -y \
    && apt-get install -y --no-install-recommends \
          ca-certificates \
          curl \
          lsb-release \
      && apt-get clean -y \
      && apt-get autoremove -y \
      && rm -rf /tmp/* /var/tmp/* \
      && rm -rf /var/lib/apt/lists/*


RUN useradd -ms /bin/bash xteve
USER xteve
# Set Enverment Varables
ENV Version=2.1.0

WORKDIR /home/xteve
COPY --from=builder /go/src/github.com/xteve-project/xTeVe .
COPY docker-entrypoint.sh /home/xteve/


USER root
RUN mkdir /config
RUN chown xteve:xteve /config
RUN chmod +x /home/xteve/docker-entrypoint.sh
RUN chown xteve:xteve /home/xteve/docker-entrypoint.sh
USER xteve

VOLUME [/config]
VOLUME [/tmp/xteve/]
EXPOSE 34400

ENTRYPOINT ["/home/xteve/docker-entrypoint.sh"]