FROM rustedwizard/gcc:latest

ARG OPENSSL_VERSION=openssl-1.1.1h
ARG CMAKE_VERSION=3.18.4

ENV PATH=/cmake/bin:/openssl/bin:$PATH
ENV OPENSSL_ROOT_DIR=/openssl

RUN yum update -y \
    ; yum install wget perl-core zlib-devel -y\
    ; wget https://www.openssl.org/source/${OPENSSL_VERSION}.tar.gz\
    ; wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz \
    ; tar -xf ${OPENSSL_VERSION}.tar.gz \
    ; tar -xf cmake-${CMAKE_VERSION}.tar.gz \
    ; rm -f ${OPENSSL_VERSION}.tar.gz \
    ; rm -f cmake-${CMAKE_VERSION}.tar.gz \
    ; cd ${OPENSSL_VERSION} \
    ; ./config --prefix=/openssl --openssldir=/openssl shared zlib \
    ; make -j 16 \
    ; make test \ 
    ; make install \
    ; cd .. \
    ; rm -rf /${OPENSSL_VERSION} \
    ; touch ${OPENSSL_VERSION}.conf \
    ; echo "/openssl/lib" >> /${OPENSSL_VERSION}.conf \
    ; mv /${OPENSSL_VERSION}.conf /etc/ld.so.conf.d \
    ; ldconfig -v \
    ; cd cmake-${CMAKE_VERSION} \
    ; ./bootstrap --prefix=/cmake -- -DCMAKE_BUILD_TYPE:STRING=Release \
    ; make -j 16 \
    ; make install \
    ; cd .. \
    ; rm -rf /cmake-${CMAKE_VERSION} \
    ; yum remove wget -y \
    ; yum clean all

CMD [ "bash" ]
