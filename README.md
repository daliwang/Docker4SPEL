# ELM-Docker- E3SM, ELM, SPEL, HPC Docker Project: 
Docker Image Generation and Configuration for implementing custom compilers, as well as GPU and NVHPC utilization on DGX Station or Cloud Deployment.
See Wiki for full description and background

SPEL- Software for Porting ELM using compiler directives: 
    1) NVHPC Baseos & Docker is Ubuntu AMD64 based that uses NVIDIA SDK Base image to be GPU Utilized in a DGX Machine
    2) Base & Docker is Redhat ARM64 based, primarily for CPU processing
 
 
Docker Layer Architecture: AMD x64_86
1- nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04
    DGX Machine NVIDIA GPU utilizing docker image
2- fje1223/spel_base_nvhpc
    initial dependencies & config, python & pip3, expat XML parser
3- yuanfornl/elm_docker_2022
    HDF5 & netCDF
4- fje1223/spel_docker_nvhpc
    SPEL repository & software necessary dependencies
        How To: 
            1. Run Image with -i flag to execute commands from within the running container:
                    sudo nvidia-docker run -t -i fje1223/spel_docker_nvhpc
            2. Create the module test: 
                    python 3 UnitTestforELM.py
            3. cd SPEL_OpenACC/unit-tests/<the module you're attempting to create>
            4. Change the compiler flag in makefile: 
                    From: FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS) \
                    To:   FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS) \
                    make clean
                    make
            5. cd SPEL_OpenACC/unit-tests/<the module directory created from step 3>
            6. Copy reference/input data (E3SM_constants.txt):
                    cp ../../*.txt . 
            7. Run your created module using x sets of 42:
                    ./elmtest.exe <x>
