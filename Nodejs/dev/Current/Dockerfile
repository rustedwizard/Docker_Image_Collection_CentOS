FROM rustedwizard/centos:7

    #specify nodejs version and yarn version needed
    #change follwoing two variable value will change
    #version of nodejs and yarn installed
ENV NODE_VERSION=12.7.0 \
    YARN_VERSION=1.17.3

    #add user
RUN groupadd --gid 1000 node \
    ; useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
    #upgrade presinatlled package
    ; yum upgrade -y \
    #installed needed tools
    ; yum install -y curl wget gnupg epel-release git \
    #download nodejs and yarn
    ; curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
    ; curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
    #decompress nodejs and yarn into target folder
    ; tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
    ; tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
    #clean up installation file
    ; rm "node-v$NODE_VERSION-linux-x64.tar.xz" \
    ; rm yarn-v$YARN_VERSION.tar.gz \
    #wire up links so it can be used in bash
    ; ln -s /usr/local/bin/node /usr/local/bin/nodejs \
    ; ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
    ; ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg \
    #clean up yum cache
    ; yum clean all \
    #update npm to latest version
    ; npm update -g npm

CMD ["/bin/bash"]