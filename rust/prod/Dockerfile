FROM rustedwizard/centos:7

#set up rust path
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=1.36.0

    #update yum database
RUN yum update -y \
    #install needed tools
    ; yum install -y wget \
    #download rust installation file
    ; wget "https://static.rust-lang.org/rustup/archive/1.18.3/x86_64-unknown-linux-gnu/rustup-init" \
    #change file permission
    ; chmod +x rustup-init \
    #install rust
    ; ./rustup-init -y --no-modify-path --default-toolchain $RUST_VERSION \
    #remove installation file
    ; rm rustup-init \
    #change rust path folder permission
    ; chmod -R a+w $RUSTUP_HOME $CARGO_HOME \
    #remove unneeded tools
    ; yum remove -y wget \
    #cleanup yum
    ; yum clean all

CMD ["/bin/bash"]