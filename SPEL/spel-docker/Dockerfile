# Base image
FROM fje1223/spel-base:latest

LABEL maintainer.name="Franklin J Eaglebarger" \
      maintainer.email="fye@ornl.gov" \
      author.name="Franklin J Eaglebarger" \
      author.email="fye@ornl.gov" \
      description="NVHPC-based image loaded with necessary dependencies for building HLM/HLM-FATES images"

# Install dependencies
RUN yum update -y && yum install -y \
    git \
    make \
    python3 \
    python3-pip \
    gcc \
    cuda-toolkit \
    && yum clean all

# Set working directory
WORKDIR /app

# Clone SPEL repository
RUN git clone https://github.com/peterdschwartz/SPEL_OpenACC.git .

# Set environment variables
ENV FC=nvc
ENV CUDA_HOME=/usr/local/cuda

# Set up virtual environment
RUN python3 -m venv venv
RUN venv/bin/pip install --upgrade pip

# Install dependencies
# RUN venv/bin/pip install <dependency1> <dependency2> <dependency3> ...

# Set entrypoint command
CMD ["/bin/bash"]
