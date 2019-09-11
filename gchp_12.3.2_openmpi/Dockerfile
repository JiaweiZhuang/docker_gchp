FROM spack_gcc_mpi
LABEL maintainer="Jiawei Zhuang <jiaweizhuang@g.harvard.edu>"

# Git tag or commit
ARG GC_VERSION=12.3.2
ARG GCHP_VERSION=12.3.2
ARG TOP_DIR='/gchp'
ARG RUN_DIR='gchp_standard'

COPY ./gchp.gcc_openmpi.env $TOP_DIR/gchp.env

# https://github.com/geoschem/gchp/issues/40
RUN ln -s $(spack location -i hdf5)/lib/* $(spack location -i netcdf)/lib/

# Otherwise will fail at linking '-lz'
RUN yum install -y zlib-devel

WORKDIR $TOP_DIR

RUN git clone https://github.com/geoschem/geos-chem.git Code.GCHP \
    && cd Code.GCHP && git checkout $GC_VERSION \
    && git clone https://github.com/geoschem/gchp.git GCHP \
    && cd GCHP && git checkout $GCHP_VERSION \
    && mkdir -p /ExtData/HEMCO \
    && cd Run && printf "/ExtData \n 2 \n 1 \n $TOP_DIR \n $RUN_DIR \n n" | ./createRunDir.sh

WORKDIR $TOP_DIR/$RUN_DIR

RUN ln -s $TOP_DIR/gchp.env gchp.env && make build_all
