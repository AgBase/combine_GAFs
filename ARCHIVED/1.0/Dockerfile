## Dockerfile
FROM ubuntu:16.04
MAINTAINER Amanda Cooksey	
LABEL Description="combine GAF outputs from GOanna and InterProscan"

# Install all the updates and download dependencies
RUN apt-get update && apt-get install -y git

ADD combine_gafs /usr/bin

# Change the permissions and the path for the script
RUN chmod +x /usr/bin/combine_gafs

# Entrypoint
ENTRYPOINT ["/usr/bin/combine_gafs"]

RUN mkdir /work-dir

WORKDIR /work-dir

