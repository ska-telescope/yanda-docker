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
FROM python:3.7-slim-stretch as buildenv

RUN apt update

RUN apt-get install -y --no-install-recommends libc6-dev libffi-dev libssl-dev make g++

RUN pip3 install --install-option="--prefix=/usr/local" daliuge

##############################################################
# Create a new image based on only the executable parts of the old image
FROM ${DOCKER_REGISTRY_HOST}/${DOCKER_REGISTRY_USER}/yanda-base:latest
COPY --from=buildenv /usr/local /usr/local
RUN ldconfig
