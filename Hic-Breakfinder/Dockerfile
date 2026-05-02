# --- Stage 1: Build Environment ---
FROM ubuntu:22.04 AS builder

# Prevent prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libeigen3-dev \
    zlib1g-dev \
    wget \
    autoconf \
    automake

WORKDIR /build

# 1. Build BamTools
RUN git clone https://github.com/pezmaster31/bamtools && \
    cd bamtools && \
    mkdir build && cd build && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local .. && \
    make && make install

# 2. Build hic_breakfinder
RUN git clone https://github.com/dixonlab/hic_breakfinder.git && \
    cd hic_breakfinder && \
    ./configure CXXFLAGS="-fpermissive -O3" \
                CPPFLAGS="-I/usr/local/include/bamtools -I/usr/include/eigen3" \
                LIBS="-lbamtools -lz -lpthread" && \
    make && make install

# --- Stage 2: Final Image ---
FROM ubuntu:22.04

# Install only runtime libraries (zlib)
RUN apt-get update && apt-get install -y \
    zlib1g \
    libstdc++6 \
    && rm -rf /var/lib/apt/lists/*

# Copy the compiled binaries and libraries from the builder
COPY --from=builder /usr/local/bin/hic_breakfinder /usr/local/bin/
COPY --from=builder /usr/local/lib/libbamtools* /usr/local/lib/

# Update library cache so it can find libbamtools
RUN ldconfig

# Set the default command
ENTRYPOINT ["hic_breakfinder"]

