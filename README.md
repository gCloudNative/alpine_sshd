# alpine_sshd


## Dockerfile
```
#FROM alpine:3.7
FROM hermsi/alpine-sshd:latest

MAINTAINER YANG SEN <nuaays@gmail.com>

WORKDIR /

ENV TZ=Asia/Shanghai \
    TERM=xterm \
    LANG=en_US.UTF-8 \
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    ROOT_PASSWORD=nuaays \
    USERNAME=Wi3kY9yU2JPqv1GC


RUN echo http://mirrors.aliyun.com/alpine/v3.7/main > /etc/apk/repositories && \
    echo http://mirrors.aliyun.com/alpine/v3.7/community >> /etc/apk/repositories && \
    apk --update add openssh curl bash tzdata && \
    ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo Asia/Shanghai > /etc/timezone && \
    sed -i 's/#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/UsePAM yes/UsePAM no/' /etc/ssh/sshd_config && \
    sed -i 's/UsePrivilegeSeparation yes/UsePrivilegeSeparation no/' /etc/ssh/sshd_config  && \
    #sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd && \
    adduser -D -h /home/${USERNAME} -s /bin/bash -u 1000 ${USERNAME} && \
    echo "${USERNAME}:${ROOT_PASSWORD}" | chpasswd && \
    echo "root:${ROOT_PASSWORD}" | chpasswd && \
    rm -rf /var/cache/apk/* /tmp/*
    #rm -f /etc/ssh/ssh_host_rsa_key /etc/ssh/ssh_host_ecdsa_key && \
    #ssh-keygen -q -t rsa   -f /etc/ssh/ssh_host_rsa_key   -P '' -N '' && \
    #ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -P '' -N ''
    
```



## docker command
```
docker run -d -P --env ROOT_PASSWORD=123456 nuaays/alpine:sshd
```
