# Use the NVIDIA CUDA base image for the DGX machine
FROM nvidia/cuda:12.0.1-devel-ubi8

LABEL maintainer.name="Franklin J Eaglebarger" \
      maintainer.email="fye@ornl.gov" \
      author.name="Franklin J Eaglebarger" \
      author.email="fye@ornl.gov" \
      description="NVHPC-based image loaded with necessary dependencies for building HLM/HLM-FATES images"

# Install necessary dependencies for GPU utilization
RUN yum install -y cuda-command-line-tools-11-4 \
    libcublas-11-4 \
    libcufft-11-4 \
    libcurand-11-4 \
    libcusolver-11-4 \
    libcusparse-11-4 \
    libnpp-11-4 && \
    rm -rf /var/cache/yum/*

# Set environment variables for CUDA
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
ENV PATH=/usr/local/cuda/bin:$PATH

# Set environment variables for NVHPC
ENV NVHPC_CUDA_HOME=/usr/local/cuda
ENV PATH=/usr/local/nvidia/bin:$PATH
ENV LD_LIBRARY_PATH=/usr/local/nvidia/lib64:$LD_LIBRARY_PATH

# Set the working directory
WORKDIR /spel
