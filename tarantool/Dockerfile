# Tarantool docker file
FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y git build-essential cmake libncurses5-dev libboost-all-dev
RUN git clone https://github.com/tarantool/mysql-tarantool-replication.git /mysql-tarantool-replication
WORKDIR /mysql-tarantool-replication
RUN git submodule update --init --recursive
RUN cmake .
RUN make

#COPY app.lua /opt/tarantool
#ENTRYPOINT ["tarantool", "/opt/tarantool/app.lua"]