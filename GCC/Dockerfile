FROM centos:7 AS BASE

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

# configure gcc source
WORKDIR /build
    # enable default language support, only compile for x86_64
RUN ./../${GCC_VERSION}/configure --prefix=/usr/gcc --enable-languages=default --disable-multilib \
    # compile the code
    ; make -j 16 \
    # install
    ; make install

# build final image
FROM centos:7 AS PROD

LABEL Maintainer="Rusted Wizard"

#set environment
ENV LD_LIBRARY_PATH=/usr/local/lib:/usr/local/lib64
ENV PATH=/usr/local/bin:/usr/gcc/bin:$PATH

#copy built binary
COPY --from=BASE /usr /usr

#install dependencies
RUN yum update -y \
    ; yum install gmp-devel mpfr-devel libmpc-devel -y 

# cleanup
WORKDIR /
RUN yum clean all 

CMD [ "bash"]