## Welcome

It is a clone from https://github.com/Eaglebarger/SPEL-Docker with modifications and test procedures on Nvidia GPUs. 

Welcome to the SPEL-Docker project, which is designed to make E3SM land model (ELM) unit testing with SPEL available to users without strenuous ELM/GPU configuration. Technically, with the power of Docker, we provide a seamless and efficient solution to simplifying software deployment and optimizing GPU utilization for ELM simulation and unit testing with SPEL. For demonstration purposes, the current SPEL package only contains one unit testing module (LakeTemperature) targeting Nvidia GPUs using NVHPC/OpenACC.

## Introduction

## SPEL: Software for Porting ELM using Compiler Directives
SPEL is an innovative toolkit developed to automate port and optimize ELM code on GPU (currently with OpenACC) within a Function Unit Test Framework. SPEL Origination/Documentation: [SPEL_OpenACC](https://github.com/peterdschwartz/SPEL_OpenACC)

### Docker Layer Architecture: x86-64

1. `nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04`
   - Base Nvidia-docker image that supports Nvidia GPUs. The NVHPC Baseos & Docker is an Ubuntu x86-64-based image that contains the NVIDIA SDK for Nvidia GPU.

2. `yuanfornl/elm_baseos_nvhpc:23.5`
   - Prepare the previous Nvidia-docker image for ELM (Added Python/pip3, XML parser, etc)

3. `yuanfornl/elm_baseos_nvhpc:latest`
   - Added HDF5, netCDF, netCDF-Fortran, OpenMPI

4. `fje1223/spel_docker_nvhpc`
   - Added SPEL repository & software necessary dependencies
     
5. `wangdl1108/docker4spel_demo`
   - Added Reference for code validation

### How To Run SPEL in the customized docker image
*(Also available in the Wiki)*

1. Assuming Docker CLI is installed and running, pull the image:
   ```
   sudo docker pull wangdl1108/docker4spel_demo:latest
   ```

2. Run the image with the -i flag to execute commands from 
   within the running container:

   (CPU-version)
   ```
   sudo nvidia-docker run -t -i wangdl1108/docker4spel_demo:latest
   ```

   (GPU-version)  (e.g., using the first GPU(0) in the system )
   ```
   sudo nvidia-docker run --gpus 0 -t -i wangdl1108/docker4spel_demo:latest
   ```

3. Change the Directory to scripts if not already in it:
   ```
   cd SPEL_OpenACC/scripts
   ```

4. Create the module test: (e.g., A LakeTemperature unit test case will be created)
   ```
   python3 UnitTestforELM.py
   ```

5. Change Directory to the Test Module that you're attempting to run:
   ```
   cd SPEL_OpenACC/unit-tests/<your module> (e.g., LakeTemperature)
   ```

6. Create the unit test application
  (CPU version)
   ```
   make clean; make
   ```
  (GPU version) 
   Change the compiler flag in the makefile:
   - From: `FC_FLAGS = $(FC_FLAGS_DEBUG) $(MODEL_FLAGS) \`
   - To: `FC_FLAGS = $(FC_FLAGS_ACC) $(MODEL_FLAGS) \`
   Then
   ```
   make clean; make
   ```

7. Stay in the working directory:
   ```
   cd SPEL_OpenACC/unit-tests/<the module directory created from step 5>  (e.g., LakeTemparature)
   ```

8. Copy the reference/input data (E3SM_constants, object_files, output_vars) for unit testing:
   ```
   cp ../../*.txt .
   ```

9. Run your created module over a group of land cells (using x sets of 42 AmeriFlux sites):
   ```
   ./elmtest.exe <x>  (e.g., 2 means 84 land cells (2 groups of 42 AmeriFlux sites))
   ```
   
10. (Extra) Save LakeTemperatuter result for verification.
    
    Add "call update_vars_LakeTemperature(gpuflag, "Test")" into the main.F90 (line 232)
         see ../../LakeTemperature_main_verification.F90 main.F90
    Then
    ```
    make
    ./elmtest.exe 1
    ```
    Depending on the flag you choose, it will produce
      cpu_LakeTemperatureTest.txt or   cpu_LakeTemperatureTest.txt

    ```
    python ../../scripts/errorAnalysis.py -c cpu_LakeTemperatureRef.txt
          -g <cpu/gpu>_LakeTemperature.txt
    ```
   
    
      

