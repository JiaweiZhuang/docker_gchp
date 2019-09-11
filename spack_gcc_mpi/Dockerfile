FROM spack_base
LABEL maintainer="Jiawei Zhuang <jiaweizhuang@g.harvard.edu>"

ARG GCC_VERSION="8.3.0"

RUN spack -v install gcc@${GCC_VERSION}
RUN spack compiler add $(spack location -i gcc@${GCC_VERSION})

# Install NetCDF/HDF5 without MPI dependency
RUN spack -v install netcdf-fortran %gcc@${GCC_VERSION} ^netcdf~mpi ^hdf5~mpi+fortran+hl

RUN spack -v install openmpi %gcc@${GCC_VERSION}
RUN spack -v install intel-mpi %gcc@${GCC_VERSION}
