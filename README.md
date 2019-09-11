Build base images (needed by all later builds):

    cd docker_gchp

    docker build -t spack_base ./spack_base

    docker build -t spack_gcc_mpi ./spack_gcc_mpi

Compile GCHP (each can be built independently):

    docker build -t gchp_12.3.2_openmpi ./gchp_12.3.2_openmpi

    docker build -t gchp_12.5.0_openmpi ./gchp_12.5.0_openmpi
