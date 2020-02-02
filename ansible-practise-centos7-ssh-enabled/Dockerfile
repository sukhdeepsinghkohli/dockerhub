FROM centos:7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]

# ------------------------------------------------------------------------------
# Set image metadata
# ------------------------------------------------------------------------------
LABEL \
	maintainer="Sukhdeep Singh Kohlie <sukhdeepsinghkohli@gmail.com>" 

# ------------------------------------------------------------------------------
# Install the Packages
# ------------------------------------------------------------------------------
RUN yum update -y glibc-common
RUN yum install -y sudo passwd openssh-server openssh-clients tar screen crontabs strace telnet perl libpcap bc patch ntp dnsmasq unzip pax

# ------------------------------------------------------------------------------
# ADD EPEL repository
# ------------------------------------------------------------------------------
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

# ------------------------------------------------------------------------------
# start sshd to generate host keys, patch sshd_config and enable yum repos
# ------------------------------------------------------------------------------
RUN (sed -i 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config; \
     sed -i 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config; \
     sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config; \
     sed -i 's/enabled=0/enabled=1/' /etc/yum.repos.d/CentOS-Base.repo)

RUN (mkdir -p /root/.ssh/; \
     echo "StrictHostKeyChecking=no" > /root/.ssh/config; \
     echo "UserKnownHostsFile=/dev/null" >> /root/.ssh/config)

# ------------------------------------------------------------------------------
# Set root user's password = passw0rd 
# ------------------------------------------------------------------------------
RUN echo "root:passw0rd" | chpasswd
RUN (systemctl enable sshd.service;)

EXPOSE 22
CMD ["/usr/sbin/init"]
