FROM centos:7

ENV GOLANG_VERSION 1.15

    #update yum database
RUN yum update -y \
    #install needed tools
    ; yum install make glibc-devel gcc gcc-c++ wget pkgconfig git gnupg -y \
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

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH" \
    #install go tools for development
    ; go get -x -d github.com/stamblerre/gocode 2>&1 \
    ; go build -o gocode-gomod github.com/stamblerre/gocode \
    ; mv gocode-gomod $GOPATH/bin/ \
    ; go get -u -v github.com/ramya-rao-a/go-outline \
    ; go get -u -v github.com/acroca/go-symbols \
    ; go get -u -v github.com/mdempsky/gocode \
    ; go get -u -v github.com/rogpeppe/godef \
    ; go get -u -v golang.org/x/tools/cmd/godoc \
    ; go get -u -v github.com/zmb3/gogetdoc \
    ; go get -u -v golang.org/x/lint/golint \
    ; go get -u -v github.com/fatih/gomodifytags \
    ; go get -u -v golang.org/x/tools/cmd/gorename \
    ; go get -u -v sourcegraph.com/sqs/goreturns \
    ; go get -u -v golang.org/x/tools/cmd/goimports \
    ; go get -u -v github.com/cweill/gotests/... \
    ; go get -u -v golang.org/x/tools/cmd/guru \
    ; go get -u -v github.com/josharian/impl \
    ; go get -u -v github.com/haya14busa/goplay/cmd/goplay \
    ; go get -u -v github.com/uudashr/gopkgs/cmd/gopkgs \
    ; go get -u -v github.com/davidrjenni/reftools/cmd/fillstruct \
    ; go get -u -v github.com/alecthomas/gometalinter \
    ; go get -u -v github.com/golangci/golangci-lint/cmd/golangci-lint \
    ; go get -u -v github.com/mgechev/revive \
    ; go get -u -v github.com/derekparker/delve/cmd/dlv 2>&1 \
    #add user
    ; groupadd --gid 1000 go \
    ; useradd -s /bin/bash --uid 1000 --gid 1000 -m go

WORKDIR ${GOPATH}/bin

RUN ./gometalinter --install

WORKDIR ${GOPATH}

CMD [ "/bin/bash" ]