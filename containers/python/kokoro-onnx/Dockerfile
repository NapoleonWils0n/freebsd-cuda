#===============================================================================
# FILE: Dockerfile
# Project: kokoro-onnx
#===============================================================================


#===============================================================================
# Inherit from your universal, pre-built FreeBSD base image
#===============================================================================

FROM localhost/base-image:latest


#===============================================================================
# freebsd host user passed on the command line
#===============================================================================

ARG HOST_USER


#===============================================================================
# LD_LIBRARY_PATH
#===============================================================================

ENV LD_LIBRARY_PATH=/usr/lib64:/usr/lib64/nvidia-host


#==============================================================================
# Install Build Tools for Compiling Python Native Modules
#==============================================================================

RUN dnf install -y --allowerasing \
    python3.12 \
    python3.12-devel \
    python3.12-pip \
    gcc \
    gcc-c++ \
    git \
    pkgconf-pkg-config \
    zlib \
    libxml2 \
    glibc-langpack-en \
    libsndfile \
    libffi-devel \
    && dnf clean all

# Install uv globally inside the container image layer
RUN pip3.12 install --break-system-packages uv


#===============================================================================
# Drop down to your matching host user context
#===============================================================================

USER ${HOST_USER}
