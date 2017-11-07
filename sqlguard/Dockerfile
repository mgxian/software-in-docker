FROM registry.cn-shanghai.aliyuncs.com/mgxian/centos7:base
MAINTAINER will<will835559313@163.com>


# 安装SQLAdvisor
RUN yum install -y git m4 make cmake libaio-devel libffi-devel glib2 glib2-devel gcc gcc-c++ && \
wget http://ftp.gnu.org/gnu/bison/bison-2.6.5.tar.xz && \
tar xf bison-2.6.5.tar.xz && cd bison-2.6.5 && \
./configure && make && make install && bison -V && \
cd / && git clone https://github.com/Meituan-Dianping/SQLAdvisor.git && \
yum install -y http://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm && \
yum install -y Percona-Server-shared-56 && \
cd /usr/lib64/ && ln -s libperconaserverclient_r.so.18 libperconaserverclient_r.so || \
cd /SQLAdvisor && ldconfig -v | grep libperconaserverclient && \
CPUS=$(cat /proc/cpuinfo | grep processor | wc -l) && \
cmake -DBUILD_CONFIG=mysql_release -DCMAKE_BUILD_TYPE=debug -DCMAKE_INSTALL_PREFIX=/usr/local/sqlparser ./ && \
make -j $CPUS && make install && cd sqladvisor/ && cmake -DCMAKE_BUILD_TYPE=debug ./ && make -j $CPUS && \
cp sqladvisor /usr/local/bin/ && sqladvisor --help && \

# 安装inception
cd / && git clone https://github.com/mysql-inception/inception.git && \
yum install -y cmake  libncurses5-dev libssl-devg++ openssl-devel.x86_64 ncurses-devel && \
cd /inception && sh inception_build.sh debug && \
cp debug/sql/Inception /usr/local/bin/ && Inception --help && \

# 删除无用的文件
rm -rf /inception /SQLAdvisor
