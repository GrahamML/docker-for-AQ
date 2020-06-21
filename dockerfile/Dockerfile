ARG RT=20.03
# Use an NVIDIA TensorRT official docker image
#   Refer to the following URL for available tags.
#   https://docs.nvidia.com/deeplearning/tensorrt/container-release-notes/running.html#running
FROM nvcr.io/nvidia/tensorrt:${RT}-py3
ARG RT

ENV DEBCONF_NOWARNINGS yes
ENV DEBIAN_FRONTEND noninteractive
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE DontWarn

# Update & Upgrade Ubuntu
RUN apt-get -y update \
    && apt-get -y upgrade

# Set the working directory to /workspace
WORKDIR /workspace

# Copy the AQ archive into the container at /workspace
COPY AQ_linux.tar.gz /workspace

# Make AQ
RUN tar xvfz AQ_linux.tar.gz \
    && rm *gz \
    && cd AQ \
    && make \
    && cd .. \
    && rm README.md \
    && rm -r tensorrt

# Remove /var/chace
RUN rm -rf /var/cache/*
