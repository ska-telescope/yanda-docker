#    ICRAR - International Centre for Radio Astronomy Research
#    (c) UWA - The University of Western Australia, 2019
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

RUN apt-get update

# Needed for building casacore & oskar
RUN apt-get install -y \
    git \
    cmake \
    flex \
    bison \
    gfortran \
    git \
    g++ \
    make

# Needed for casacore
RUN apt-get install -y \
    libboost-dev \
    libboost-all-dev \
    libcfitsio-dev \
    libfftw3-dev \
    libgsl-dev \
    libopenblas-dev \
    libopenmpi-dev \
    libpython-dev \
    python-numpy \
    wcslib-dev

#############################################################
# Build casacore
WORKDIR /home
RUN git clone https://bitbucket.csiro.au/scm/askapsdp/yandasoft.git

# Casacore need these, but they aren't installed by default
WORKDIR /home/yandasoft
RUN ./build_all.sh -s ubuntu -c -j $COMPILE_JOBS -C -DUSE_READLINE=OFF

#############################################################
# Build OSKAR
WORKDIR /home
RUN git clone https://github.com/OxfordSKA/OSKAR.git

WORKDIR /home/OSKAR
RUN cmake -DCMAKE_CXX_COMPILER=g++ -DCMAKE_C_COMPILER=gcc .
RUN make -j$COMPILE_JOBS
RUN make install

#############################################################
# Add the SPEAD2 into the default python env making sure the
# code is installed in /usr/local/lib/python3
RUN pip3 install spead2

##############################################################
# Create a new image based on only the executable parts of the old image
FROM ${DOCKER_REGISTRY_HOST}/${DOCKER_REGISTRY_USER}/yanda-daliuge:latest
RUN apt-get install -y \
    libboost-dev \
    libboost-all-dev \
    libcfitsio-dev \
    libfftw3-dev \
    libgsl-dev \
    libopenblas-dev \
    libopenmpi-dev \
    libpython-dev \
    python-numpy \
    wcslib-dev
COPY --from=buildenv /usr/local /usr/local
RUN ldconfig
