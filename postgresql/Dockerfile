FROM rustedwizard/base:centos

     #specify gosu version
ENV GOSU_VERSION=1.11

    #add users for postgresql
RUN groupadd -r postgres --gid=1000 \
    ; useradd -r -g postgres --uid=1000 --home-dir=/var/lib/postgresql --shell=/bin/bash postgres \
    #directory for initdb
    ; mkdir /docker-entrypoint-initdb.d \
    #update yum database
    ; yum update -y \
    #install wget
    ; yum install -y wget \
    #download GOSU
    ; wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    #change permission for gosu
    ; chmod +x /usr/local/bin/gosu \
    #running gosu
    ; gosu nobody true \
    #since postgresql already provide repo for postgresql 11 download, so this image will just take it and use it
    ; yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm -y \
    #install postgresql component
    ; yum install postgresql11 postgresql11-server -y \
    #clean up yum database
    ; yum clean all \
    #make all neccsary directories for postgresql
    ; mkdir -p /var/run/postgresql && chown -R postgres:postgres /var/run/postgresql && chmod 2777 /var/run/postgresql \
    ; mkdir -p /var/lib/postgresql \
    #change permission
    ; chown -R postgres:postgres /var/lib/postgresql \
    #modify settings of postgresql.
    ; sed -ri "s!^#?(listen_addresses)\s*=\s*\S+.*!\1 = '*'!" /usr/pgsql-11/share/postgresql.conf.sample \
    ; grep -F "listen_addresses = '*'" /usr/pgsql-11/share/postgresql.conf.sample

    #set environment variable
ENV PATH=$PATH:/usr/pgsql-11/bin \
    PGDATA=/var/lib/postgresql/data

    #create data directory
RUN mkdir -p "$PGDATA" && chown -R postgres:postgres "$PGDATA" && chmod 777 "$PGDATA"

       #specify volume
       #if data persistence is needed this is the directory a volume should map to
VOLUME /var/lib/postgresql/data

     #copy start up shell script
COPY docker-entrypoint.sh /usr/local/bin/

            #specify entry point
ENTRYPOINT ["docker-entrypoint.sh"]

       #expose port
EXPOSE 5432

CMD ["postgres"]