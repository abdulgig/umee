FROM ubuntu:22.04
LABEL maintainer="abdul@umee.cc"

# Prepare Environment
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true
ENV GO_VERSION=1.20

RUN set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
    wget \
  	curl \
    net-tools \
	jq \
    tree \
    tmux \
    gpg-agent \
    iputils-ping \
    iproute2 \
    openssh-server \
    software-properties-common \
    vim \
    ;

# Service Initialization
RUN mkdir -p /umee/node_settings/
WORKDIR /umee

# Lets setup go
RUN add-apt-repository ppa:longsleep/golang-backports -y && apt-get -y update
RUN apt-get install golang-${GO_VERSION} -y && apt-get install golang-go -y

# Lets get wasmvm
RUN wget --no-check-certificate https://raw.githubusercontent.com/CosmWasm/wasmvm/v1.1.1/internal/api/libwasmvm.$(uname -m).so -O /lib/libwasmvm.$(uname -m).so
COPY node* /umee/
COPY settings/* /opt/

ENTRYPOINT ["./node_init"]
EXPOSE 26656 9090 1317
