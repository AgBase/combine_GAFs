## Dockerfile
FROM ubuntu:22.04
LABEL Description="combine GAF outputs from GOanna and InterProscan. iprs_pathannot_to_gmt.py will make a gmt file of IPRS, KEGG and FlyBase domain and pathways annotations."

ENV DEBIAN_FRONTEND=noninteractive

# Install all the updates and download dependencies
RUN apt-get update && apt-get install -y \
    python3-scipy \
    python3-sklearn \
    python3-numpy \
    python3-biopython \
    pip \
    wget

RUN pip3 install pandas

ADD combine_gafs /usr/bin
ADD iprs_pathannot_to_gmt.py /usr/bin

# Change the permissions and the path for the script
RUN chmod +x /usr/bin/combine_gafs
RUN chmod +x /usr/bin/iprs_pathannot_to_gmt.py

ENV PATH=$PATH:/usr/bin

# Entrypoint
#ENTRYPOINT ["/usr/bin/iprs_pathannot_to_gmt.py"]

RUN mkdir /work-dir

WORKDIR /work-dir

