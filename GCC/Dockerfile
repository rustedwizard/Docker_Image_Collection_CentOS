FROM centos:7 AS BASE

LABEL Maintainer="Rusted Wizard"

ARG GCC_VERSION=gcc-10.2.0
ARG MAKE_VERSION=make-4.3

# install dependency
RUN yum update -y \
    ; yum groupinstall "Development tools" -y \
    ; yum install wget gmp-devel mpfr-devel libmpc-devel -y\ 
    # get latest GCC source code
    ; wget https://bigsearcher.com/mirrors/gcc/releases/${GCC_VERSION}/${GCC_VERSION}.tar.gz \
    ; wget https://ftp.gnu.org/gnu/make/${MAKE_VERSION}.tar.gz \
    ; tar -xf ${MAKE_VERSION}.tar.gz \
    ; tar -xf ${GCC_VERSION}.tar.gz \
    ; rm -f ${GCC_VERSION}.tar.gz

# build and install latest version make
WORKDIR /${MAKE_VERSION}

RUN ./configure \
    ; make \
    ; make install 

WORKDIR /

RUN yum remove wget make -y \
    ; rm -rf ${MAKE_VERSION} \
    ; rm -f ${MAKE_VERSION}.tar.gz \
    ; ln /usr/local/bin/make /bin/make

# configure source
WORKDIR /build
    # enable language support for C/C++ Fortran and Go, only compile for x86_64
RUN ./../${GCC_VERSION}/configure --prefix=/usr/gcc --enable-languages=default --disable-multilib \
    # compile the code
    ; make -j 16 \
    # install
    ; make install

FROM centos:7 AS PROD

ENV LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64
ENV PATH=/usr/local/bin:/usr/gcc/bin:$PATH

COPY --from=BASE /usr /usr

RUN yum update -y \
    ; yum install gmp-devel mpfr-devel libmpc-devel -y 

# cleanup
WORKDIR /
RUN yum clean all 

CMD [ "bash"]