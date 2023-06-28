## Welcome

Welcome to the SPEL-Docker project! We are dedicated to simplifying software deployment and optimizing GPU utilization for your E3SM, ELM, and SPEL workflows. With the power of Docker, we provide a seamless and efficient solution to enhance your research and development processes. Explore the possibilities and elevate your projects to new heights.

# SPEL-Docker: ELM, E3SM, HPC Docker Project

**Written by:** Franklin Eaglebarger  
**Directed by:** Dr. Dali Wang  
**Assisted by:** Fengming Yuan, Chris Layton, and Peter Schwartz  

**Conducted at:** Oak Ridge National Laboratory (ORNL) in conjunction with Oak Ridge Institute for Science and Education (ORISE) and Pellissippi State Community College (PSCC)  
**Summer 2023**

## Introduction

The SPEL-Docker project focuses on Docker image generation and configuration, enabling the implementation of custom compilers, GPU utilization, and NVHPC on DGX Stations or in cloud deployments. This project aims to provide a seamless environment for running the SPEL application on both AMD-based CPU machines and DGX machines, with a particular emphasis on GPU utilization. The Docker images and containers are prebuilt and readily available through Docker Desktop and GitHub, providing easy access to all the necessary components.

One of the main objectives of this project is to consolidate and organize the layers present in the SPEL ecosystem, as well as clarify and streamline the connections between SPEL, ELM, OLMT, and E3SM. By doing so, we aim to create a comprehensive resource that encompasses all the required information and tools for researchers and developers.

In the broader context, the SPEL application serves as a crucial component in the larger picture. It leverages the power of GPUs on DGX machines to enhance the capabilities of the E3SM land model (ELM). SPEL builds upon previous work by Dali Wang and Yao Cindy, offering a robust method for developing ELM onto GPUs. The SPEL repository contains the necessary Fortran source files, GPU-ready ELM test modules, Python scripts, and optimized versions of source files.

Furthermore, Fengming Yuan has developed a Docker application version of the ELM containers, complete with comprehensive instructions for running ELM, including the utilization of Jupyter notebooks for visualizing the ELM output. This provides an additional layer of convenience and accessibility for ELM users.

ELM itself builds off the Offline Land Model Testbed (OLMT), a set of Python scripts designed to automate offline land model simulations. OLMT enables simulations at various scales, from single sites to global regions, and facilitates the creation, building, and submission of the necessary cases for a full land model BGC (Biogeochemical) simulation. It automates the generation of surface and domain files based on existing global files.

By combining these components and streamlining the deployment process with Docker, the SPEL-Docker project aims to empower researchers and developers in their exploration of the E3SM, ELM, SPEL, and OLMT ecosystems.

**Please refer to the Wiki for a full Description/Background Information, Getting Docker, Docker Image Configuration, Creating SPEL Application, and Acronym/Definition Glossary.**

## References

Please find below the references related to the components and tools used in this project:

- [ELM Containers](https://github.com/fmyuan/elm_containers)
- [E3SM](https://github.com/fmyuan/E3SM)
- [SPEL_OpenACC](https://github.com/peterdschwartz/SPEL_OpenACC)
- [OLMT](https://github.com/FASSt-simulation/OLMT)
- [Simulation Containers](https://github.com/FASSt-simulation/simulation_containers)

## SPEL: Software for Porting ELM using Compiler Directives

The NVHPC Baseos & Docker is an Ubuntu AMD64-based image that utilizes the NVIDIA SDK Base image for GPU utilization in a DGX Machine.

### Docker Layer Architecture: AMD x86-64

1. `nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04`
   - DGX Machine NVIDIA GPU utilizing Docker image

2. `fje1223/spel_base_nvhpc`
   - Initial dependencies & configuration, Python/pip3, expat XML parser

3. `yuanfornl/elm_docker_2022`
   - HDF5, netCDF, OpenMPI

4. `fje1223/spel_docker_nvhpc`
   - SPEL repository & software necessary dependencies
   - SPEL Origination/Documentation: [SPEL_OpenACC](https://github.com/peterdschwartz/SPEL_OpenACC)

### How To Run SPEL
*(Also available in the Wiki)*

1. Assuming Docker CLI is installed and running, pull the image:
   ```
   sudo docker pull fje1223/spel_docker_nvhpc:latest
   ```

2. Run the image with the -i flag to execute commands from 
   within the running container:
   ```
   sudo nvidia-docker run -t -i fje1223/spel_docker_nvhpc:latest
   ```

3. Change Directory to scripts if not already in it:
   ```
   cd SPEL_OpenACC/scripts
   ```

4. Create the module test:
   ```
   python3 UnitTestforELM.py
   ```

5. Change Directory to the Test Module that you're attempting to run:
   ```
   cd SPEL_OpenACC/unit-tests/<your module>
   ```

6. Change the compiler flag in the makefile:
   - From: `FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS) \`
   - To: `FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS) \`
   ```
   make clean
   make
   ```

6. Change the directory to the working directory:
   ```
   cd SPEL_OpenACC/unit-tests/<the module directory created from step 5>
   ```

7. Copy the reference/input data (E3SM_constants.txt):
   ```
   cp ../../*.txt .
   ```

8. Run your created module using x sets of 42:
   ```
   ./elmtest.exe <x>
   ```

## Processes

1. Docker Background & E3SM Info (ELM, SPEL)
2. Docker Custom Image: ELM requirements - Python, expat, HDF5, netCDF
   - Origination Documentation on and running ELM: [ELM Containers Wiki](https://github.com/fmyuan/elm_containers/wiki)
3. Docker with GPU Utilization for DGX
   - [NVIDIA HPC SDK](https://developer.nvidia.com/hpc-sdk)
4. New Image with SPEL Dependencies/Repo