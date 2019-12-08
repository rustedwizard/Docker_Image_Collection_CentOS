FROM rustedregistry.azurecr.io/centos:latest

ENV GOLANG_VERSION 1.13.5

    #update yum database
RUN yum update -y \
    #install needed tools
    ; yum install make glibc-devel gcc gcc-c++ wget pkgconfig -y \
    #clean up yum
    ; yum clean all \
    #download GoLang
    ; wget -O go.tgz "https://golang.org/dl/go${GOLANG_VERSION}.linux-amd64.tar.gz" \
    #install GoLang
    ; tar -C /usr/local -xzf go.tgz \
    #Remove installation file
    ; rm go.tgz \
    #set path temperaily for test
    ; export PATH="/usr/local/go/bin:$PATH" \
    ; go version

ENV GOPATH=/go \
    PATH=$GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"

WORKDIR ${GOPATH}

CMD [ "/bin/bash" ]