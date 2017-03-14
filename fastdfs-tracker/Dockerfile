#base on docker centos 
FROM centos:7

#AUTHOR
MAINTAINER 21jhf <21jhf@163.com>

#安装wget
#RUN yum install -y wget

#更换yum源 
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
ADD CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo
#	&& wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo \
RUN yum clean all \
	&& yum makecache 
	
#安装工具集
RUN yum install -y zlib zlib-devel pcre pcre-devel gcc gcc-c++ openssl openssl-devel libevent libevent-devel perl unzip net-tools git wget

	
#配置环境变量
ENV CENTOS_VERSION=7 \
	FASTDFS_PATH=/fastdfs \
    FASTDFS_TRACKER_PATH=/fastdfs/tracker \
    FASTDFS_VERSION=5.09 \
    NGINX_VERSION=1.11.7 
	
#创建必要的目录
RUN mkdir -p ${FASTDFS_PATH}/download \
 && mkdir -p ${FASTDFS_TRACKER_PATH} \
 && mkdir -p ${FASTDFS_TRACKER_PATH}/logs 
 
#下载
WORKDIR ${FASTDFS_PATH}/download/
RUN git clone https://github.com/happyfish100/libfastcommon.git
RUN git clone https://github.com/happyfish100/fastdfs.git
RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -P ${FASTDFS_PATH}/download/nginx 
RUN tar zxvf ${FASTDFS_PATH}/download/nginx/nginx-${NGINX_VERSION}.tar.gz -C ${FASTDFS_PATH}/download/nginx 

#安装libfastcommon
WORKDIR ${FASTDFS_PATH}/download/libfastcommon/
RUN ["/bin/bash", "-c", "./make.sh && ./make.sh install"]

#安装fastdfs
WORKDIR ${FASTDFS_PATH}/download/fastdfs/
RUN ["/bin/bash", "-c", "./make.sh && ./make.sh install"] 

#安装nginx
WORKDIR ${FASTDFS_PATH}/download/nginx/nginx-${NGINX_VERSION}
RUN ./configure --prefix=/usr/local/nginx \
 && make \
 && make install
 
#添加初始化配置
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
COPY fdfs_config/* /etc/fdfs/
COPY nginx_config/nginx.conf /usr/local/nginx/conf/nginx.conf
COPY ["start","stop","${FASTDFS_PATH}/"]
RUN chmod +x ${FASTDFS_PATH}/start \
  && chmod +x ${FASTDFS_PATH}/stop \
  && ln -s ${FASTDFS_PATH}/start /usr/local/bin/start_fdfs \ 
  && ln -s ${FASTDFS_PATH}/stop /usr/local/bin/stop_fdfs

EXPOSE 22122 80

VOLUME ["$FASTDFS_PATH", "/etc/fdfs"]

CMD ["bash","start_fdfs"] 