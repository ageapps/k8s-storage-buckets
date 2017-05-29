FROM debian:jessie-slim

MAINTAINER Adrián García <age.apps.dev@gmail.com>

RUN apt-get update && apt-get install -y lsb-release && apt-get install -y curl

WORKDIR /gcsfuse 

# COPY ./credentials . 

ENV FUSE_MOUNT_DIR=/content
ENV FUSE_BUCKET_NAME=fuse-test-bucket

RUN export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s` && echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | tee /etc/apt/sources.list.d/gcsfuse.list

RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

RUN apt-get update && apt-get install -y gcsfuse

COPY setUp.sh .
