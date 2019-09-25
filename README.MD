## coreseek中文检索收藏整理
由于coreseek早已停止维护,特整理收藏coreseek相关资源供大家使用
# linux安装coreseek
## 安装依赖包
yum -y install glibc make gcc g++ gcc-c++ libtool autoconf automake imake mysql-devel libxml2-devel expat-devel

## 首先解压进入coreseek目录
tar -zxvf coreseek-3.2.14.tar.gz
cd coreseek-3.2.14
## 安装mmseg分词
cd mmseg-3.2.14
 
./bootstrap
 
./configure --prefix=/usr/local/mmseg3
 
make && make install
警告不用理会
#### 测试mmseg分词
/usr/local/mmseg/bin/mmseg -d /usr/local/mmseg/etc (测试文档路径)

## 安装coreseek
cd csft-3.2.14 
sh buildconf.sh 
./configure –prefix=/usr/local/coreseek –without-unixodbc –with-mmseg –with-mmseg-includes=/usr/local/mmseg/include/mmseg/ –with-mmseg-libs=/usr/local/mmseg/lib/ –with-mysql 
make && make install

#### 安装方法网上有很多就不列出了,写出容易发生的错误

- 编译coreseek的./configure加参数中间会报错
查看error错误行号,

将所有的:
T val = ExprEval ( this-m_pArg, tMatch );
交换成
T val = this-ExprEval ( this-m_pArg, tMatch );

- 修改 configure 文件把 #define USE_LIBICONV 0 最后的数值由1改为0

解决上面两个错误,基本已经没有问题可以正常运行了

## 重建索引:
/usr/local/coreseek/bin/indexer -c etc/csft.conf –all
## 启动coreseek服务
usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft.conf
停止服务:
/usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft/conf --stop

默认端口号9312,windows安装更简单,解压cmd直接运行.

更多详细信息:[查看这篇安装文章]("https://blog.csdn.net/qq_35118483/article/details/78410987")
