FROM centos:7

ARG GCC_VERSION=gcc-10.2.0
ARG MAKE_VERSION=make-4.3

RUN yum update -y \
    ; yum groupinstall "Development tools" -y \
    ; yum install wget -y\ 
    ; wget https://bigsearcher.com/mirrors/gcc/releases/${GCC_VERSION}/${GCC_VERSION}.tar.gz \
    ; wget https://ftp.gnu.org/gnu/make/${MAKE_VERSION}.tar.gz \
    ; tar -xf ${MAKE_VERSION}.tar.gz \
    ; tar -xf ${GCC_VERSION}.tar.gz \
    ; rm -f ${GCC_VERSION}.tar.gz \
    ; cd ${MAKE_VERSION} \
    ; ./configure \
    ; make \
    ; make install \
    ; cd .. \
    ; cd ${GCC_VERSION} \
    ; ./contrib/download_prerequisites \
    ; cd .. \
    ; mkdir build \
    ; cd build \
    ; ./../${GCC_VERSION}/configure --enable-languages=default --disable-multilib \
    ; make -j 16 \
    ; make install \
    ; cd .. \
    ; rm -rf /${GCC_VERSION} \
    ; rm -rf /${MAKE_VERSION} \
    ; rm -rf build \
    ; rm -f ${MAKE_VERSION}.tar.gz \
    ; yum group remove "Development tools" -y \
    ; yum clean all -y \
    ; echo '/usr/local/lib64' > /etc/ld.so.conf.d/local-lib64.conf \
    ; ldconfig -v

ENV PATH=/usr/bin:${PATH}

CMD [ "bash" ] 