FROM centos:7

ENV GOSU_VERSION=1.12 \
    JSYAML_VERSION=3.14.0

    #add user
RUN groupadd -r mongodb \
    ; useradd -r -g mongodb mongodb

    #update yum database
RUN yum update -y \
    #install dependency
    ; yum install epel-release -y \
    #install needed tools and required libararies
    ; yum install jq wget numactl glibc-devel openssl gnupg -y

    #install neccessary tool from internet
RUN wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    ; chmod +x /usr/local/bin/gosu \
    ; gosu --version \
    ; gosu nobody true \
    ; wget -O /js-yaml.js "https://github.com/nodeca/js-yaml/raw/${JSYAML_VERSION}/dist/js-yaml.js" \
    ; mkdir /docker-entrypoint-initdb.d

     #add mongodb repo
COPY mongodb-org-4.4.repo /etc/yum.repos.d

    #install mongodb
RUN yum install -y mongodb-org \
    #change configure for container
    ; mv /etc/mongod.conf /etc/mongod.conf.orig \
    #create neccessary dat folder
    ; mkdir -p /data/db /data/configdb \
    #change folder permission
    ; chown -R mongodb:mongodb /data/db /data/configdb \
    #clean up
    ; yum clean all

VOLUME /data/db /data/configdb

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 27017

CMD ["mongod"]