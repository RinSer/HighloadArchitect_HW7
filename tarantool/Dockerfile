# Tarantool docker file
FROM centos:7.5.1804

RUN yum -y update; yum clean all
RUN yum -y install systemd; yum clean all;
RUN yum -y install git ncurses-devel make cmake gcc-c++ boost boost-devel wget unzip nano bzip2 mysql-devel mysql-lib
RUN git clone https://github.com/tarantool/mysql-tarantool-replication.git /mysql-tarantool-replication
WORKDIR /mysql-tarantool-replication
RUN git submodule update --init --recursive
RUN wget --no-check-certificate -P /mysql-tarantool-replication/lib/libslave/include http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
RUN cmake .
RUN make
RUN sed -i 's/replicatord\.cfg/replicatord\.yml/g' replicatord.service
RUN cp replicatord /usr/local/sbin/replicatord
RUN cp replicatord.service /etc/systemd/system
RUN curl -L https://tarantool.io/zyxgyPc/release/2/installer.sh | bash
RUN yum -y install tarantool
COPY replicatord.yml /usr/local/etc/replicatord.yml
COPY replica.lua /etc/tarantool/instances.available/replica.lua
WORKDIR /etc/tarantool
RUN mkdir instances.enabled
RUN ln -s /instances.available/replica.lua instances.enabled
RUN systemctl enable tarantool@replica
RUN systemctl enable replicatord
CMD ["/usr/sbin/init"]