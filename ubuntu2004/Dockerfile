FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt update
RUN apt upgrade -y

# Install basic tools
RUN apt install -y --no-install-recommends \
    apt-transport-https \
    apt-utils \
    ca-certificates \
    curl \
    git \
    iputils-ping \
    jq \
    lsb-release \
    software-properties-common

# Install AZCLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Az DevOps agent
ENV TARGETARCH=linux-x64
WORKDIR /azp
COPY ./start.sh .
RUN chmod +x start.sh

ENTRYPOINT [ "./start.sh" ]
