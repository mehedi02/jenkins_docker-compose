#FROM jenkins/jenkins:lts-jdk11
# FROM jenkins/jenkins:2.319.1-jdk11

# USER root
# #RUN apt-get update && apt-get install libc6 && apt-get install -y docker
# RUN apt-get update

# #install kubectl
# RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
# RUN chmod u+x kubectl && mv kubectl /usr/local/bin/kubectl



### Second version

  
FROM jenkins/jenkins:alpine

USER root

# Update apk repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/main" > /etc/apk/repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories
RUN echo "http://dl-2.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

# Install the latest Docker and docker-compose binaries
RUN apk -U --no-cache \
	--allow-untrusted add \
    gcc \
    docker \
    py-pip musl-dev python3-dev libffi-dev openssl-dev libc-dev make cargo \
    && pip install --upgrade pip \
    && pip install docker-compose \
    && apk add --update -t deps curl \
    && curl -L https://storage.googleapis.com/kubernetes-release/release/v$KUBE_VERSION/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    && rm -rf /var/cache/* \
    && mkdir /var/cache/apk