FROM rustedwizard/gcc:latest AS BUILD

ARG OPENSSL_VERSION=openssl-1.1.1g
ARG CMAKE_VERSION=3.18.2

RUN yum update -y \
    ; yum install wget perl-core zlib-devel -y\
    ; wget https://www.openssl.org/source/${OPENSSL_VERSION}.tar.gz\
    ; wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz \
    ; tar -xf ${OPENSSL_VERSION}.tar.gz \
    ; tar -xf cmake-${CMAKE_VERSION}.tar.gz

# cmake requires openssl installed, build openssl first
WORKDIR ${OPENSSL_VERSION}
RUN ./config --prefix=/openssl --openssldir=/openssl shared zlib \
    ; make -j 16 \
    ; make test \ 
    ; make install 

# set up openssl
WORKDIR /
RUN touch ${OPENSSL_VERSION}.conf \
    ; echo "/openssl/lib" >> /${OPENSSL_VERSION}.conf \
    ; mv /${OPENSSL_VERSION}.conf /etc/ld.so.conf.d \
    ; ldconfig -v
ENV PATH=/openssl/bin:$PATH
ENV OPENSSL_ROOT_DIR=/openssl

# start compile cmake
WORKDIR /cmake-${CMAKE_VERSION}
RUN ./bootstrap --prefix=/cmake -- -DCMAKE_BUILD_TYPE:STRING=Release \
    ; gmake -j 16 \
    ; make install

FROM centos:7 AS PROD

ARG OPENSSL_VERSION=openssl-1.1.1g

COPY --from=BUILD /openssl /openssl
COPY --from=BUILD /cmake /cmake
COPY --from=BUILD /usr/gcc/lib64 /cmake_lib64

RUN touch ${OPENSSL_VERSION}.conf \
    ; echo "/openssl/lib" >> /${OPENSSL_VERSION}.conf \
    ; mv /${OPENSSL_VERSION}.conf /etc/ld.so.conf.d \
    ; echo '/cmake_lib64' > /etc/ld.so.conf.d/local-lib64.conf \
    ; ldconfig -v
    
ENV OPENSSL_ROOT_DIR=/openssl
ENV PATH=/openssl/bin:/cmake/bin:$PATH

CMD [ "bash" ]
