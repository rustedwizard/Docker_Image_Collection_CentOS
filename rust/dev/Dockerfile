FROM centos:7

#for dev container only install powershell core
ARG PS_VERSION=7.0.3
ARG PACKAGE_VERSION=7.0.3
ARG PS_PACKAGE=powershell-${PACKAGE_VERSION}-1.rhel.7.x86_64.rpm
ARG PS_PACKAGE_URL=https://github.com/PowerShell/PowerShell/releases/download/v${PS_VERSION}/${PS_PACKAGE}

#set up rust path
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.46.0

#PowerShell core related ENV
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    # set a fixed location for the Module analysis cache
    PSModuleAnalysisCachePath=/var/cache/microsoft/powershell/PSModuleAnalysisCache/ModuleAnalysisCache

    #update yum database
RUN yum update -y \
    ; yum upgrade -y \
    #install needed tools
    ; yum install -y wget gnupg git gssntlmssp less \
    #add neccessary user
    ; groupadd --gid 1000 rust \
    ; useradd --uid 1000 --gid rust --shell /bin/bash --create-home rust \
    #download rust installation file
    ; wget "https://static.rust-lang.org/rustup/archive/1.22.1/x86_64-unknown-linux-gnu/rustup-init" \
    #download Powershell Core
    ; curl -sSL ${PS_PACKAGE_URL} -o /tmp/powershell.rpm \
    #install and config Powershell Core
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
          }" \
    #change rust installation file permission
    ; chmod +x rustup-init \
    #install rust
    ; ./rustup-init -y --no-modify-path --profile complete --default-toolchain $RUST_VERSION \
    #remove installation file
    ; rm rustup-init \
    #change rust path folder permission
    ; chmod -R a+w $RUSTUP_HOME $CARGO_HOME \
    #cleanup yum
    ; yum clean all

#set user
ENV USER=rust

CMD ["pwsh"]