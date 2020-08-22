FROM centos:7

    #specify nodejs version and yarn version needed
    #change follwoing two variable value will change
    #version of nodejs and yarn installed
ENV NODE_VERSION=10.16.1 

    #add user
RUN groupadd --gid 1000 node \
    ; useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
    #upgrade presinatlled package
    ; yum upgrade -y \
    #installed needed tools
    ; yum install -y curl wget gnupg epel-release git \
    #download nodejs and yarn
    ; curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
    #decompress nodejs and yarn into target folder
    ; tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
    #clean up installation file
    ; rm "node-v$NODE_VERSION-linux-x64.tar.xz" \
    #wire up links so it can be used in bash
    ; ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    #clean up yum cache
    ; yum clean all \
    #update npm to latest version
    ; npm update -g npm \
    #install latest version of yarn
    ; npm install -g yarn  

#copy startup script and set entrypoint
COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]
#set command to use
CMD ["node"]