# Dockerfile for CUDA-enabled llama-swap on FreeBSD
#
# Please note that as of this writing this requires nvidia-driver-devel on the
# FreeBSD side.

# First we inherit from the base image from NapoleonWils0n/freebsd-cuda on github
#
FROM localhost/base-image:latest

# Set to a git branch, tag, or commit hash to set llama.cpp version
ARG LLAMACPP_REFSPEC=master

# Set to a llama-swap release number
ARG LLAMA_SWAP_VERSION=244

# Library path
#
ENV LD_LIBRARY_PATH=/usr/lib64:/usr/lib64/nvidia-host

# Build dependencies.  We'll build llama.cpp from source with CUDA support.
#

RUN dnf config-manager --add-repo https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo
RUN dnf install -y cmake clang git cuda --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin --exclude=gstreamer1*

# Build llama.cpp
#

RUN cd /tmp && git clone https://github.com/ggml-org/llama.cpp.git
RUN cd /tmp/llama.cpp && git checkout ${LLAMACPP_REFSPEC} && env CUDACXX=/usr/local/cuda-13.3/bin/nvcc cmake -B build -DCMAKE_INSTALL_PREFIX=/usr -DGGML_CUDA=ON -DGGML_CUDA_USE_FLASH_ATTENTION=ON -DGGML_CUDA_FA_ALL_QUANTS=ON && cmake --build build --config Release -j
RUN cd /tmp/llama.cpp/build && cmake --install .

# Pull llama-swap FreeBSD binary from github releases.
# This is a static FreeBSD binary.
#

RUN curl -L -o /tmp/llama-swap.tar.gz https://github.com/mostlygeek/llama-swap/releases/download/v${LLAMA_SWAP_VERSION}/llama-swap_${LLAMA_SWAP_VERSION}_freebsd_amd64.tar.gz
RUN cd /tmp && file llama-swap.tar.gz && tar zxvf llama-swap.tar.gz && mv llama-swap /usr/bin

# Remove build dependencies
#

# Copy some nvidia libs that aren't in the FreeBSD pkg.
#
RUN cp /usr/local/cuda-13.3/targets/x86_64-linux/lib/lib{cublas,cudart}* /usr/lib64
RUN chmod a+rx /usr/lib64/lib*.so* && chmod a+r /usr/lib64/lib*.a

RUN dnf remove -y cmake clang git cuda
RUN dnf autoremove -y && dnf clean all
RUN rm -rf /tmp/*

# We need to run as non-root because LD_PRELOAD doesn't work with uid 0
#
USER nobody

ENTRYPOINT /usr/bin/env LD_PRELOAD=/usr/lib64/dummy-uvm.so llama-swap --config /config/config.yaml --listen 0.0.0.0:${LLAMA_SWAP_PORT:-8080}
