# Ubuntu based NVIDIA SDK NVHPC GPU Utilization
FROM fje1223/spel_baseos_nvhpc:latest

LABEL maintainer.name="Franklin J Eaglebarger" \
      maintainer.email="fye@ornl.gov" \
      author.name="Franklin J Eaglebarger" \
      author.email="fye@ornl.gov" \
      description="NVHPC-based image loaded with necessary dependencies for building HLM/HLM-FATES images"

# Set the working directory
WORKDIR /app

# Clone the SPEL repository
RUN git clone https://github.com/peterdschwartz/SPEL_OpenACC.git

# Install additional dependencies required by SPEL
RUN apt-get update && \
    apt-get install -y \
    make \
#    cuda-toolkit \
#    lapack \
    netcdf-bin \
    libnetcdf-dev

# Set the environment variables for NVHPC and CUDA
ENV PATH="/opt/nvidia/hpc_sdk/bin:${PATH}"
ENV LD_LIBRARY_PATH="/opt/nvidia/hpc_sdk/lib:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/usr/local/cuda/lib64:${LD_LIBRARY_PATH}"
ENV LD_LIBRARY_PATH="/usr/local/cuda/extras/CUPTI/lib64:${LD_LIBRARY_PATH}"

# Configure SPEL
RUN cd /app/SPEL_OpenACC && \
    sed -i 's/FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS)/FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS)/' Makefile

# Build the SPEL unit test module
RUN cd /app/SPEL_OpenACC/scripts && \
    python3 UnitTestforELM.py

# Set the entrypoint to run the unit test
ENTRYPOINT ["/app/SPEL_OpenACC/unit-tests/LakeTemperature/elmtest.exe"]
