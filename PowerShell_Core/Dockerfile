FROM centos:7

ARG PS_VERSION=7.0.3
ARG PACKAGE_VERSION=7.0.3
ARG PS_PACKAGE=powershell-${PACKAGE_VERSION}-1.rhel.7.x86_64.rpm
ARG PS_PACKAGE_URL=https://github.com/PowerShell/PowerShell/releases/download/v${PS_VERSION}/${PS_PACKAGE}

# Define ENVs for Localization/Globalization

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    # set a fixed location for the Module analysis cache
    PSModuleAnalysisCachePath=/var/cache/microsoft/powershell/PSModuleAnalysisCache/ModuleAnalysisCache

# Install dependencies and clean up

RUN curl -sSL ${PS_PACKAGE_URL} -o /tmp/powershell.rpm \
    ; yum install -y /tmp/powershell.rpm \
    # Required for gssntlmssp
    ; yum install -y epel-release \
    # Update now that we have epel-release
    ; yum update -y \
    # Install libraries for NTLM support
    ; yum install -y \
      gssntlmssp \
      # less is required for help in powershell
      less \
    ; yum upgrade -y \
    ; yum clean all \
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
          }"

CMD ["pwsh"]