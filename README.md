# ELM-Docker: E3SM, ELM, SPEL, HPC Docker Project

Docker Image Generation and Configuration for implementing custom compilers, GPU, and NVHPC utilization on DGX Station or Cloud Deployment.

**Please refer to the Wiki for a full Description/Background Information, Getting Docker, Docker Image Configuration, Creating SPEL Application, and Acronym/Definition Glossary.**

## SPEL: Software for Porting ELM using Compiler Directives

The NVHPC Baseos & Docker is an Ubuntu AMD64-based image that utilizes the NVIDIA SDK Base image for GPU utilization in a DGX Machine.

### Docker Layer Architecture: AMD x64_86

1. `nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04`
   - DGX Machine NVIDIA GPU utilizing Docker image

2. `fje1223/spel_base_nvhpc`
   - Initial dependencies & configuration, Python/pip3, expat XML parser

3. `yuanfornl/elm_docker_2022`
   - HDF5, netCDF, OpenMPI

4. `fje1223/spel_docker_nvhpc`
   - SPEL repository & software necessary dependencies
   - SPEL Origination/Documentation: [SPEL_OpenACC](https://github.com/peterdschwartz/SPEL_OpenACC)

### How To Run SPEL:
*Also in the WIKI*

1. Assuming Docker CLI is installed and running, pull the image:
   ```
   sudo docker pull fje1223/spel_docker_nvhpc:latest
   ```

2. Run the image with the -i flag to execute commands from within the running container:
   ```
   sudo nvidia-docker run -t -i fje1223/spel_docker_nvhpc
   ```

3. Create the module test:
   ```
   python3 UnitTestforELM.py
   ```

4. Change the compiler flag in the makefile:
   - From: `FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS) \`
   - To: `FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS) \`
   ```
   make clean
   make
   ```

5. Change the directory to the working directory:
   ```
   cd SPEL_OpenACC/unit-tests/<the module directory created from step 3>
   ```

6. Copy the reference/input data (E3SM_constants.txt):
   ```
   cp ../../*.txt .
   ```

7. Run your created module using x sets of 42:
   ```
   ./elmtest.exe <x>
   ```

## Processes:

1. Docker Background & E3SM Info (ELM, SPEL)
2. Docker Custom Image: ELM requirements - Python, expat, HDF5, netCDF
   - Origination Documentation on and running ELM: [ELM Containers Wiki](https://github.com/fmyuan/elm_containers/wiki)
3. Docker with GPU Utilization for DGX
   - [NVIDIA HPC SDK](https://developer.nvidia.com/hpc-sdk)
4. New Image with SPEL Dependencies/Repo

Welcome to the ELM-Docker project, where we harness the power of Docker to simplify software deployment, optimize GPU utilization, and enable efficient cloud deployment. Explore the possibilities and take your E3SM, ELM, and SPEL workflows to new heights!