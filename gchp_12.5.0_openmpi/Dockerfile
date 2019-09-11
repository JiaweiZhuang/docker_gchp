FROM spack_gcc_mpi
LABEL maintainer="Jiawei Zhuang <jiaweizhuang@g.harvard.edu>"

# Git tag or commit
ARG GC_VERSION=12.5.0
ARG GCHP_VERSION=12.5.0
ARG TOP_DIR='/gchp'
ARG RUN_DIR='gchp_standard'

COPY ./gchp.gcc_openmpi.env $TOP_DIR/gchp.env

WORKDIR $TOP_DIR

# https://github.com/geoschem/gchp/issues/40
RUN ln -s $(spack location -i hdf5)/lib/* $(spack location -i netcdf)/lib/
RUN yum install -y zlib-devel

# neede CMake for gFTL
RUN yum install -y cmake

# Build GMAO FTL for 12.5.0+
RUN git clone https://github.com/Goddard-Fortran-Ecosystem/gFTL \
    && cd gFTL && git checkout v1.0.0 \
    && mkdir build && cd build \
    && cmake .. -DCMAKE_INSTALL_PREFIX=../install \
    && make install

RUN git clone https://github.com/geoschem/geos-chem.git Code.GCHP \
    && cd Code.GCHP && git checkout $GC_VERSION \
    && git clone https://github.com/geoschem/gchp.git GCHP \
    && cd GCHP && git checkout $GCHP_VERSION \
    && mkdir -p /ExtData/HEMCO \
    && cd Run && printf "/ExtData \n $TOP_DIR/gFTL/install \n 2 \n 1 \n $TOP_DIR \n $RUN_DIR \n n" | ./createRunDir.sh

WORKDIR $TOP_DIR/$RUN_DIR

RUN ln -s $TOP_DIR/gchp.env gchp.env && make build_all
