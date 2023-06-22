# ------------------------------------------------------------------------
# Debian baseOS docker container to use with ELM/CTSM builds
# AMD64 version
# Docker File Code Base from gcc850ompi316 multiarch. Base Image from NVIDIA. HPC SDK.
# No hdf5, No netCDF, No OpenMPI
# ------------------------------------------------------------------------

FROM nvcr.io/nvidia/nvhpc:23.5-devel-cuda_multi-ubuntu22.04
LABEL maintainer.name="Franklin J Eaglebarger" \
      maintainer.email="fye@ornl.gov" \
      author.name="Franklin J Eaglebarger" \
      author.email="fye@ornl.gov" \
      description="NVHPC-based image loaded with necessary dependencies for building HLM/HLM-FATES images"

# Set a few variables that can be used to control the docker build
# TODO: actually use these to allow for argument builds OR
# remove if its not relevant
# ARG OPENMPI_VERSION=3.1.6
ARG EXPAT_VERSION=2.4.7
ARG HDF5_VERSION=1.12.1
ARG NETCDF_VERSION=4.8.1
ARG NETCDF_FORTRAN_VERSION=4.5.4

# set path variables
ENV PATH=/usr/local/bin:$PATH
ENV LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
ENV PATH=/usr/local/hdf5/bin:$PATH
ENV PATH=/usr/local/netcdf/bin:$PATH
ENV LD_LIBRARY_PATH=/usr/local/hdf5/lib:$LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH=/usr/local/netcdf/lib:$LD_LIBRARY_PATH
ENV HDF5_DIR=/usr/local/hdf5

# Update the system and install initial dependencies
RUN apt-get update -y && \
    apt-get install -y \
    cmake \
    git \
    subversion \
    bzip2 \
    libgmp3-dev \
    m4 \
    wget \
    libcurl4-openssl-dev \
    zlib1g-dev \
    libncurses5-dev \
    libxml2 \
    libxml2-dev \
    csh \
    liblapack-dev \
    libblas-dev \
    liblapack-dev \
    libxml-libxml-perl \
    libxml2-utils \
    vim \
    libudunits2-0 \
    libudunits2-dev \
    udunits-bin \
    python3 \
    python3-dev \
    python3-pip \
    apt-utils \
    ftp \
    apt-transport-https \
    libssl-dev \
    openssl \
    libncurses5-dev \
    libsqlite3-dev \
    gsl-bin \
    libgsl-dev \
    flex \
    nco \
    locales && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean all

# Add symbolic link python to python3
RUN ln -sf /usr/bin/python3 /usr/bin/python

## Install program to configure locales
RUN apt-get update && \
    apt-get install -y locales && \
    echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && \
    locale-gen

## Set default locale for the environment
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

## Check locales
RUN locale -a

## Compile Expat XML parser
RUN cd / && \
    wget https://github.com/libexpat/libexpat/releases/download/R_2_4_7/expat-2.4.7.tar.bz2 && \
    tar -xvjf expat-2.4.7.tar.bz2 && \
    cd expat-2.4.7 && \
    ./configure && \
    make && \
    make install && \
    cd / && \
    rm -rf expat-2.4.7 && \
    rm expat-2.4.7.tar.bz2

## HDF5 - use alternative source since main HDF5 source doesn't have useful download links
#https://support.hdfgroup.org/ftp/HDF5/releases/
#RUN cd / \
#    && mkdir -p /usr/local/hdf5 \
#    && wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.12/hdf5-1.12.1/src/hdf5-1.12.1.tar.gz \
#    && tar -zxvf hdf5-1.12.1.tar.gz \
#    && cd hdf5-1.12.1 \
#    && CC=mpicc ./configure --enable-fortran --enable-parallel --prefix=/usr/local/hdf5 \
#    && make \
#    && make install \
#    && cd / \
#    && rm -rf hdf5-1.12.1 \
#    && rm hdf5-1.12.1.tar.gz

## Install netCDF & netCDF-Fortran
#RUN cd / \
#    && mkdir -p /usr/local/netcdf \
#    && wget https://downloads.unidata.ucar.edu/netcdf-c/$NETCDF_VERSION/netcdf-c-$NETCDF_VERSION.tar.gz \
#    && tar -zxvf netcdf-c-$NETCDF_VERSION.tar.gz \
#    && cd netcdf-c-$NETCDF_VERSION \
#    && export H5DIR=/usr/local/hdf5 \
#    && export NCDIR=/usr/local/netcdf \
#    && CC=mpicc CPPFLAGS=-I${H5DIR}/include LDFLAGS=-L${H5DIR}/lib ./configure --enable-parallel-tests --prefix=${NCDIR} \
#    && make \
#    && make install \
#    && cd / \
#    && rm -rf netcdf-c-$NETCDF_VERSION \
#    && rm netcdf-c-$NETCDF_VERSION.tar.gz \
#    && wget https://downloads.unidata.ucar.edu/netcdf-fortran/$NETCDF_FORTRAN_VERSION/netcdf-fortran-$NETCDF_FORTRAN_VERSION.tar.gz \
#    && tar -zxvf netcdf-fortran-$NETCDF_FORTRAN_VERSION.tar.gz \
#    && cd netcdf-fortran-$NETCDF_FORTRAN_VERSION \
#    && export NFDIR=/usr/local/netcdf \
#    && FC=mpif90 CPPFLAGS=-I${NCDIR}/include LDFLAGS=-L${NCDIR}/lib ./configure --prefix=${NFDIR} --enable-parallel-tests \
#    && make \
#    && make install \
#    && cd / \
#    && rm -rf netcdf-fortran-$NETCDF_FORTRAN_VERSION \
#    && rm netcdf-fortran-$NETCDF_FORTRAN_VERSION.tar.gz

## Install python libraries for OLMT
RUN apt-get update && apt-get install -y libopenmpi-dev

RUN pip3 install wheel numpy scipy netCDF4 h5py configparser cftime
RUN CFLAGS=-noswitcherror pip install mpi4py

RUN ln -sf /usr/bin/python3 /usr/bin/python
### EOF