FROM centos:latest
MAINTAINER "The CentOS Project" <admin@jiobxn.com>
ARG LATEST="0"

RUN \cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN yum clean all; yum -y install epel-release; yum -y update; yum install openvpn --downloadonly --downloaddir=/mnt/; yum -y install openvpn easy-rsa squid cronie iptables net-tools bash-completion vim wget \
        && yum clean all

RUN \cp -R /usr/share/easy-rsa/ /etc/openvpn/ \
        && cd /mnt && rpm2cpio openvpn-*.rpm |cpio -imd \
        && \cp /mnt/usr/share/doc/openvpn-*/sample/sample-config-files/{server.conf,client.conf} /etc/openvpn/ \
        && rm -rf /mnt/*

RUN mkdir /var/run/openvpn

VOLUME /key

COPY openvpn.sh /openvpn.sh
RUN chmod +x /openvpn.sh

WORKDIR /usr/sbin

ENTRYPOINT ["/openvpn.sh"]

EXPOSE 1194 80

CMD ["openvpn", "--writepid", "/var/run/openvpn/server.pid", "--cd", "/etc/openvpn/", "--config", "/etc/openvpn/server.conf"]

# docker build -t openvpn .
# docker run -d --restart always --privileged -v /docker/openvpn:/key -p 1194:1194 --hostname openvpn --name openvpn openvpn 
# cat /docker/openvpn/openvpn.log
