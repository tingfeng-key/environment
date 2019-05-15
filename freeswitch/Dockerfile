FROM ubuntu:xenial

ARG TZ=Asia/Shanghai
ENV TZ ${TZ}

#sed -i 's/httpredir.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
######## ubuntu国内源 ########
#sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

RUN apt-get clean && apt-get update -yqq && apt-get install -y tzdata wget

######## 设置时区 ########
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && date


########  安装依赖 ########
RUN apt-get -y install libtiff5-dev openssh-server vim git autoconf \
  libtool* zlib* libjpeg-dev sqlite3* libsqlite3-dev libcurl4-openssl-dev \
  libpcre3-dev  libspeexdsp-dev libldns-dev libedit-dev yasm nasm liblua5* \
  libopus-dev libsndfile1-dev
######## 安装CMAKE ########
RUN cd /usr/local/src/ && wget https://cmake.org/files/v3.13/cmake-3.13.3.tar.gz \
  && tar -zxvf cmake-3.13.3.tar.gz && cd cmake-3.13.3 \
  && ./bootstrap && make && make install \
  && ln -sf /usr/local/src/cmake-3.13.3/bin/* /usr/bin/
######## 安装libuuid ########
RUN cd /usr/local/src/ && wget https://jaist.dl.sourceforge.net/project/libuuid/libuuid-1.0.3.tar.gz \
  && tar xzvf libuuid-1.0.3.tar.gz \
  && cd libuuid-1.0.3 && ./configure && make && make install
######## 编译libks ########
RUN cd /usr/local/src/ && git clone https://github.com/signalwire/libks.git \
  && cd libks && cmake . && make install
######## 安装signalwire-c ########
RUN cd /usr/local/src/ \
  && git clone https://github.com/signalwire/signalwire-c.git \
  && cd signalwire-c && cmake . && make && make install
######## git下载源码包 ########
#RUN git clone https://freeswitch.org/stash/scm/fs/freeswitch.git \
#    && cd freeswitch/ && ./bootstrap.sh && ./configure && make && make install

######## wget下载源码包 ########
  ### 方式一：wget从官方下载文件 ###
RUN wget http://files.freeswitch.org/freeswitch-releases/freeswitch-1.8.5.tar.xz \
  && ls -la && xz -d freeswitch-1.8.5.tar.xz && ls -la && tar -xf freeswitch-1.8.5.tar
  ### 方式二：从宿主主机拷贝 ###
#RUN cd /usr/local/src/ && cp /var/zip/freeswitch-1.8.5.tar ./ && tar -xf freeswitch-1.8.5.tar

RUN cd freeswitch-1.8.5/ && ./configure && make && make install
######## 建立软连接 ########
RUN ln -sf /usr/local/freeswitch/bin/freeswitch /usr/local/bin/ \
  && ln -sf /usr/local/freeswitch/bin/fs_cli /usr/local/bin/

RUN freeswitch -nc
#RUN cd freeswitch-1.8.5/ && make sounds-install && make moh-install \
#  && make cd-sounds-install && make cd-moh-install
