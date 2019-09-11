FROM centos:7.6.1810
LABEL maintainer="Jiawei Zhuang <jiaweizhuang@g.harvard.edu>"

# Ref:
# https://spack.readthedocs.io/en/latest/docker_for_developers.html
# https://github.com/spack/spack/blob/develop/share/spack/docker/centos-7.dockerfile

ENV DOCKERFILE_BASE=centos            \
    DOCKERFILE_DISTRO=centos          \
    DOCKERFILE_DISTRO_VERSION=7       \
    SPACK_ROOT=/opt/spack             \
    FORCE_UNSAFE_CONFIGURE=1          \
    DEBIAN_FRONTEND=noninteractive    \
    CURRENTLY_BUILDING_DOCKER_IMAGE=1 \
    container=docker

RUN yum update -y                                             \
 && yum install -y epel-release                               \
 && yum update -y                                             \
 && yum --enablerepo epel groupinstall -y "Development Tools" \
 && yum --enablerepo epel install -y                          \
        curl           findutils gcc-c++    gcc               \
        gcc-gfortran   git       gnupg2     hostname          \
        iproute        Lmod      make       patch             \
        openssh-server python    python-pip tcl               \
        unzip          which                                  \
 && pip install boto3                                         \
 && rm -rf /var/cache/yum                                     \
 && yum clean all

RUN git clone https://github.com/spack/spack.git $SPACK_ROOT

RUN ( echo ". /usr/share/lmod/lmod/init/bash"                       \
 &&   echo ". \$SPACK_ROOT/share/spack/setup-env.sh"                \
 &&   echo "if [ \"\$CURRENTLY_BUILDING_DOCKER_IMAGE\" '!=' '1' ]"  \
 &&   echo "then"                                                   \
 &&   echo "  . \$SPACK_ROOT/share/spack/spack-completion.bash"     \
 &&   echo "fi"                                                   ) \
       >> /etc/profile.d/spack.sh                                   \
 && ( echo "f=\"\$SPACK_ROOT/share/spack/docker/handle-ssh.sh\""    \
 &&   echo "if [ -f \"\$f\" ]"                                      \
 &&   echo "then"                                                   \
 &&   echo "  .  \"\$f\""                                           \
 &&   echo "else"                                                   \
 &&   cat  $SPACK_ROOT/share/spack/docker/handle-ssh.sh             \
 &&   echo "fi"                                                   ) \
       >> /etc/profile.d/handle-ssh.sh                              \
 && ( echo "f=\"\$SPACK_ROOT/share/spack/docker/handle-prompt.sh\"" \
 &&   echo "if [ -f \"\$f\" ]"                                      \
 &&   echo "then"                                                   \
 &&   echo "  .  \"\$f\""                                           \
 &&   echo "else"                                                   \
 &&   cat  $SPACK_ROOT/share/spack/docker/handle-prompt.sh          \
 &&   echo "fi"                                                   ) \
       >> /etc/profile.d/handle-prompt.sh                           \
 && mkdir -p /root/.spack                                           \
 && cp $SPACK_ROOT/share/spack/docker/modules.yaml                  \
        /root/.spack/modules.yaml                                   \
 && rm -rf /root/*.* /run/nologin $SPACK_ROOT/.git

WORKDIR /root
SHELL ["/bin/bash", "-l", "-c"]

# TODO: add a command to Spack that (re)creates the package cache
RUN spack spec netcdf-fortran

ENTRYPOINT ["/bin/bash", "/opt/spack/share/spack/docker/entrypoint.bash"]
CMD ["docker-shell"]
