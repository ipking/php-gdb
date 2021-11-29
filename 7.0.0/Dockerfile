FROM centos:latest

# Install dependencies
RUN yum install -y gdb tar gzip wget gcc gcc-c++ glibc automake autoconf libtool make libxml2 libxml2-devel


RUN cd /home/  \
    && wget https://www.php.net/distributions/php-7.0.0.tar.gz --no-check-certificate\ 
    && tar -zxf php-7.0.0.tar.gz \
    && rm php-7.0.0.tar.gz

RUN cd /home/php-7.0.0 \
    && ./configure --enable-phpdbg-debug --enable-debug \
    && make \
    && find -type f -name '*.a' -delete \
    && make install

# Install debuginfo
RUN sed -i 's/enabled=0/enable=1/g' /etc/yum.repos.d/CentOS-Linux-Debuginfo.repo
RUN yum -y debuginfo-install glibc-2.28-164.el8.x86_64
RUN yum -y debuginfo-install libxcrypt-4.1.1-6.el8.x86_64 libxml2-2.9.7-9.el8_4.2.x86_64 xz-libs-5.2.4-3.el8.x86_64 zlib-1.2.11-17.el8.x86_64

# Set workdir
WORKDIR /home/
