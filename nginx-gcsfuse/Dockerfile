FROM debian:jessie-slim

MAINTAINER Adrián García <age.apps.dev@gmail.com>
# Install needed tools
RUN apt-get update && apt-get install -y lsb-release && apt-get install -y curl

# COPY ./credentials . 

WORKDIR /gcsfuse 

ENV FUSE_MOUNT_DIR=/content
ENV FUSE_BUCKET_NAME=fuse-test-bucket
# Add gcsfuse to resources.list 
RUN export GCSFUSE_REPO=gcsfuse-`lsb_release -c -s` && echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | tee /etc/apt/sources.list.d/gcsfuse.list
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

# Update and install gcsfuse
RUN apt-get update && apt-get install -y gcsfuse

RUN apt-get install -y nginx  

RUN rm -v /etc/nginx/nginx.conf

# Set custom nginx config files
COPY nginx.conf /etc/nginx/

RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Inject configurration script
COPY setUp.sh .

CMD /bin/bash ./setUp.sh && service nginx start