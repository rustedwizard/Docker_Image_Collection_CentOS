FROM centos:latest

#this is a simple docker file
#it basically pull the latest centos image
#and upgrade its package to latest
RUN yum upgrade -y \
    #cleanup yum cache
    ; yum clean all

CMD [ "/bin/bash" ]
