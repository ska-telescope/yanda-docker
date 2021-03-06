#    ICRAR - International Centre for Radio Astronomy Research
#    (c) UWA - The University of Western Australia, 2020
#    Copyright by UWA (in the framework of the ICRAR)
#    All rights reserved
#
#    This library is free software; you can redistribute it and/or
#    modify it under the terms of the GNU Lesser General Public
#    License as published by the Free Software Foundation; either
#    version 2.1 of the License, or (at your option) any later version.
#
#    This library is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
#    Lesser General Public License for more details.
#
#    You should have received a copy of the GNU Lesser General Public
#    License along with this library; if not, write to the Free Software
#    Foundation, Inc., 59 Temple Place, Suite 330, Boston,
#    MA 02111-1307  USA

ARG DOCKER_REGISTRY_USER
ARG DOCKER_REGISTRY_HOST
FROM ${DOCKER_REGISTRY_HOST}/${DOCKER_REGISTRY_USER}/yanda-daliuge:latest as buildenv

ARG COMPILE_JOBS=2
ARG YANDASOFT_BRANCH=master

RUN apt-get update

# Toolchains, etc
RUN apt-get install -y \
    git \
    cmake \
    flex \
    bison \
    gfortran \
    git \
    g++ \
    make \
    patch \
    subversion \
    wget

# Casacore/yandasoft dependencies
RUN apt-get install -y \
    libboost-dev \
    libboost-all-dev \
    libcfitsio-dev \
    libcppunit-dev \
    libfftw3-dev \
    libgsl-dev \
    liblog4cxx-dev \
    libopenblas-dev \
    libopenmpi-dev \
    libpython-dev \
    libxerces-c-dev \
    python-numpy \
    wcslib-dev

#############################################################
# Build casacore, then all yandasoft dependencies, then yandasoft itself
WORKDIR /home
RUN git clone https://bitbucket.csiro.au/scm/askapsdp/yandasoft.git
WORKDIR /home/yandasoft
RUN ./build_all.sh -s ubuntu -c -r -j $COMPILE_JOBS -C -DUSE_READLINE=OFF
WORKDIR /home
RUN git clone https://github.com/rtobar/all_yandasoft.git
WORKDIR /home/all_yandasoft
RUN ./git-do clone -b $YANDASOFT_BRANCH
RUN mkdir build
WORKDIR build
RUN cmake ..
RUN cmake --build . -j $COMILE_JOBS
RUN cmake --build . --target install -j $COMPILE_JOBS

#############################################################
# Install casacore data
RUN mkdir /usr/local/share/casacore
RUN mkdir /usr/local/share/casacore/data
WORKDIR /usr/local/share/casacore/data
RUN wget ftp://ftp.astron.nl/outgoing/Measures/WSRT_Measures.ztar
RUN mv WSRT_Measures.ztar WSRT_Measures.tar.gz
RUN gunzip WSRT_Measures.tar.gz
RUN tar -xvf WSRT_Measures.tar
RUN rm WSRT_Measures.tar

##############################################################
# Create a new image based on only the executable parts of the old image
FROM ${DOCKER_REGISTRY_HOST}/${DOCKER_REGISTRY_USER}/yanda-daliuge:latest
RUN apt-get update && apt-get install -y \
    libboost-dev \
    libboost-all-dev \
    libcfitsio-dev \
    libcppunit-dev \
    libfftw3-dev \
    libgsl-dev \
    liblog4cxx-dev \
    libopenblas-dev \
    libopenmpi-dev \
    libpython-dev \
    libxerces-c-dev \
    python-numpy \
    wcslib-dev
COPY --from=buildenv /usr/local /usr/local
RUN ldconfig
