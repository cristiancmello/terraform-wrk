FROM ubuntu:20.04

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

ARG PUID=1000
ENV PUID ${PUID}

ARG PGID=1000
ENV PGID ${PGID}


RUN apt-get update \
    && apt-get install -y \
        sudo \
        gnupg \
        software-properties-common \
        curl \
        libzip-dev zip unzip \
        apt-utils

RUN groupadd -g ${PGID} workspace \
    && useradd -l -u ${PUID} -g workspace -m workspace \
    && usermod -p "*" workspace -s /bin/bash \
    && adduser workspace sudo

RUN echo "workspace ALL=(ALL) NOPASSWD:ALL" | tee -a /etc/sudoers


RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - \
    && apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
    && apt-get update \
    && apt-get install \
        terraform


RUN terraform -help


RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog