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
 
./configure --prefix=/usr/local/mmseg
 
make && make install
警告不用理会
#### 测试mmseg分词
/usr/local/mmseg/bin/mmseg -d /usr/local/mmseg/etc (测试文档路径)

## 安装coreseek
cd csft-3.2.14


sh buildconf.sh 



./configure --prefix=/usr/local/coreseek --with-mmseg=/usr/local/mmseg --with-mmseg-includes=/usr/local/mmseg/include/mmseg/ --with-mmseg-libs=/usr/local/mmseg/lib/ --with-mysql=/usr/local/mysql


make && make install

#### 安装方法网上有很多就不列出了,写出容易发生的错误

- 编译coreseek的./configure加参数中间会报错
查看error错误行号,
修改 # cd ./src/sphinxexpr.cpp 文件
将所有的:
T val = ExprEval ( this-m_pArg, tMatch );
交换成
T val = this->ExprEval ( this-m_pArg, tMatch );

- 修改 configure 文件把 #define USE_LIBICONV 0 最后的数值由1改为0 在 7422行

解决上面两个错误,基本已经没有问题可以正常运行了
如果sh buildconf.sh最后没有生成configure脚本，且提示automake: warnings are treated as errors，可以将configure.ac中的这行

AM_INIT_AUTOMAKE([-Wall -Werror foreign])

改为

AM_INIT_AUTOMAKE([-Wall foreign])

即删掉-Werror，然后重新运行sh buildconf.sh。

## 重建索引:
/usr/local/coreseek/bin/indexer -c etc/csft.conf –all
## 启动coreseek服务
usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft.conf
停止服务:
/usr/local/coreseek/bin/searchd -c /usr/local/coreseek/etc/csft/conf --stop

默认端口号9312,windows安装更简单,解压cmd直接运行.


## 添加开机启动,需要把coreseek的searchd加到自启动服务中去

```code
#!/bin/sh
#chkconfig:2345 80 90
#description:auto_coreseek
/usr/local/coreseek/bin/searchd -c /usr/local/coreseek/bin/cs.conf

```

放到/etc/init.d/auto_coreseek.sh
然后chmod +x /etc/init.d/auto_coreseek.sh
 
然后加到开机启动中：
chkconfig --add auto_coreseek.sh
 
完事。
 
然后研究下这都是些毛意思。
chkconfig有几个等级：
0：表示关机
1：表示单用户模式
2：表示无网络链接多用户命令行模式
3：表示有网络链接多用户命令行模式
4：表示不可用情况
5：表示带图形界面的多用户模式
6：表示重新启动
 
所以chkconfig:2345就代表在2345的等级下启动这个服务
后面的80 90分别是启动优先级和关闭优先级
 
说说程序的优先级，优先级也很好理解，就是程序被CPU执行的先后顺序，此值越小有限级别越高。所以这里的启动优先级和关闭优先级的意思就是启动脚本这个进程的优先级，和关闭脚本这个进程的优先级。


更多详细信息:[查看这篇安装文章](https://blog.csdn.net/qq_35118483/article/details/78410987)
