# ELM-Docker- E3SM, ELM, SPEL, HPC Docker Project: 
Docker Image Generation and Configuration for implementing custom compilers, as well as GPU and NVHPC utilization on DGX Station or Cloud Deployment.
* See Wiki for full description and background

SPEL- Software for Porting ELM using compiler directives: 
    NVHPC Baseos & Docker is Ubuntu AMD64 based that uses NVIDIA SDK Base image to be GPU Utilized in a DGX Machine


Docker Layer Architecture: AMD x64_86 \

1- nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04
    DGX Machine NVIDIA GPU utilizing docker image

2- fje1223/spel_base_nvhpc
    initial dependencies & config, python & pip3, expat XML parser

3- yuanfornl/elm_docker_2022
    HDF5 & netCDF & OpenMPI

4- fje1223/spel_docker_nvhpc
    SPEL repository & software necessary dependencies
    SPEL Origination/Documentation: 
        https://github.com/peterdschwartz/SPEL_OpenACC
    
        How To- Run SPEL: 
            1. Assuming Docker CLI is installed and running, Pull image: 
                sudo docker pull fje1223/spel_docker_nvhpc:latest
            2. Run Image with -i flag to execute commands from within the running container:
                    sudo nvidia-docker run -t -i fje1223/spel_docker_nvhpc
            3. Create the module test: 
                    python 3 UnitTestforELM.py
            4. cd SPEL_OpenACC/unit-tests/<the module you're attempting to create>
            5. Change the compiler flag in makefile: 
                    From: FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS) \
                    To:   FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS) \
                    make clean
                    make
            6. Change directory to the working directory
                cd SPEL_OpenACC/unit-tests/<the module directory created from step 3>
            7. Copy reference/input data (E3SM_constants.txt):
                    cp ../../*.txt . 
            8. Run your created module using x sets of 42:
                    ./elmtest.exe <x>



Processes: 
1. Docker Background & E3SM Info (ELM, SPEL)
2. Docker Custom image: ELM requirements- python, expat, HDF5, netCDF
    Origination Documentation on ELM: 
    https://github.com/fmyuan/elm_containers/wiki
3. Docker with GPU utilization for DGX
    https://developer.nvidia.com/hpc-sdk
4. New Image with SPEL Repo