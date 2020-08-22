FROM centos:7

#powershell core
ARG PS_VERSION=7.0.3
ARG PACKAGE_VERSION=7.0.3
ARG PS_PACKAGE=powershell-${PACKAGE_VERSION}-1.rhel.7.x86_64.rpm
ARG PS_PACKAGE_URL=https://github.com/PowerShell/PowerShell/releases/download/v${PS_VERSION}/${PS_PACKAGE}

    #specify nodejs version needed
    #change follwoing two variable value will change
    #version of nodejs and yarn installed
ENV NODE_VERSION=12.18.3

#powershell core ENVs
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    # set a fixed location for the Module analysis cache
    PSModuleAnalysisCachePath=/var/cache/microsoft/powershell/PSModuleAnalysisCache/ModuleAnalysisCache

    #add user
RUN groupadd --gid 1000 node \
    ; useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
    ; yum update -y \
    #installed needed tools
    ; yum install -y curl wget gnupg epel-release git gssntlmssp less\
    #upgrade presinatlled package
    ; yum upgrade -y \
    #download nodejs and yarn
    ; curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
    #download powershell core
    ; curl -sSL ${PS_PACKAGE_URL} -o /tmp/powershell.rpm \
    #install powershell core
    ; yum install -y /tmp/powershell.rpm \
    ; localedef --charmap=UTF-8 --inputfile=en_US $LANG \
    ; rm /tmp/powershell.rpm \
    ; pwsh \
        -NoLogo \
        -NoProfile \
        -Command " \
          \$ErrorActionPreference = 'Stop' ; \
          \$ProgressPreference = 'SilentlyContinue' ; \
          while(!(Test-Path -Path \$env:PSModuleAnalysisCachePath)) {  \
            Write-Host "'Waiting for $env:PSModuleAnalysisCachePath'" ; \
            Start-Sleep -Seconds 6 ; \
          }"\
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

CMD ["pwsh"]