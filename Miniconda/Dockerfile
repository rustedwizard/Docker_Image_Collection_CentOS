FROM rustedregistry.azurecr.io/centos:latest

    #update yum
RUN yum update -y \
    ; yum upgrade -y \
      #install git pgp and wget
    ; yum install wget git gnupg -y \
    ; wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    ; yum clean all \
      #install miniconda
    ; chmod +x Miniconda3-latest-Linux-x86_64.sh \
    ; ./Miniconda3-latest-Linux-x86_64.sh -b -p /miniconda \
      #remove installation file
    ; rm Miniconda3-latest-Linux-x86_64.sh \
      #wire up links
    ; ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh \
      #set up conda so it will activate base envrionemnt upon container startup
    ; echo ". /miniconda/etc/profile.d/conda.sh" >> ~/.bashrc \
    ; echo "conda activate base" >> ~/.bashrc \
      #cleanup unneeded files
    ; find /miniconda/ -follow -type f -name '*.a' -delete \
    ; find /miniconda/ -follow -type f -name '*.js.map' -delete \
    ; /miniconda/bin/conda clean -afy \
      #update base envrionemnt packages
    ; /miniconda/bin/conda update -y --all \
      #cleanup conda cache
    ; /miniconda/bin/conda clean -afy

CMD ["/bin/bash"]
