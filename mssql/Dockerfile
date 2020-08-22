FROM centos:7

LABEL Maintainer="Rusted Wizard"

RUN curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo \
    ; curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo \
    ; ACCEPT_EULA=Y yum install -y mssql-server mssql-tools unixODBC-devel mssql-server-fts \
    ; yum clean all \
    ; mkdir -p /var/opt/mssql/data \
    ; chmod -R g=u /var/opt/mssql /etc/passwd

ENV PATH=${PATH}:/opt/mssql/bin:/opt/mssql-tools/bin

# enable SQL-Agent
RUN mssql-conf set sqlagent.enabled true 

EXPOSE 1433

CMD ["sqlservr", "--accept-eula", "--pid Enterprise"]
