FROM rustedwizard/centos:7

RUN curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo \
    ; curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo \
    ; ACCEPT_EULA=Y yum install -y mssql-server mssql-tools unixODBC-devel \
    ; yum clean all \
    ; mkdir -p /var/opt/mssql/data \
    ; chmod -R g=u /var/opt/mssql /etc/passwd

USER 10001

COPY uid_entrypoint /opt/mssql-tools/bin/

ENV PATH=${PATH}:/opt/mssql/bin:/opt/mssql-tools/bin

EXPOSE 1433

VOLUME [ "/var/opt/mssql/data" ]

ENTRYPOINT [ "uid_entrypoint" ]

CMD sqlservr
