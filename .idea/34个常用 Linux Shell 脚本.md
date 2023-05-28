# 34 个 常用 Linux Shell 脚本

作为一名 Linux 工程师，会写好的脚本不仅能提高工作效率，还能有更多的时间做自己的事。最近在网上冲浪的时候，也注意收集一些大佬写过的脚本，汇总整理一下，欢迎收藏，与君共勉！



### （1）用户猜数字

```bash
#!/bin/bash

# 脚本生成一个 100 以内的随机数,提示用户猜数字,根据用户的输入,提示用户猜对了,
# 猜小了或猜大了,直至用户猜对脚本结束。

# RANDOM 为系统自带的系统变量,值为 0‐32767的随机数
# 使用取余算法将随机数变为 1‐100 的随机数
num=$[RANDOM%100+1]
echo "$num"

# 使用 read 提示用户猜数字
# 使用 if 判断用户猜数字的大小关系:‐eq(等于),‐ne(不等于),‐gt(大于),‐ge(大于等于),
# ‐lt(小于),‐le(小于等于)
while :
do 
 read -p "计算机生成了一个 1‐100 的随机数,你猜: " cai  
    if [ $cai -eq $num ]   
    then     
        echo "恭喜,猜对了"     
        exit  
     elif [ $cai -gt $num ]  
     then       
            echo "Oops,猜大了"    
       else      
            echo "Oops,猜小了" 
  fi
done
```



### （2）查看有多少远程的 IP 在连接本机



```bash
#!/bin/bash

#!/bin/bash
# 查看有多少远程的 IP 在连接本机(不管是通过 ssh 还是 web 还是 ftp 都统计) 

# 使用 netstat ‐atn 可以查看本机所有连接的状态,‐a 查看所有,
# -t仅显示 tcp 连接的信息,‐n 数字格式显示
# Local Address(第四列是本机的 IP 和端口信息)
# Foreign Address(第五列是远程主机的 IP 和端口信息)
# 使用 awk 命令仅显示第 5 列数据,再显示第 1 列 IP 地址的信息
# sort 可以按数字大小排序,最后使用 uniq 将多余重复的删除,并统计重复的次数
netstat -atn  |  awk  '{print $5}'  | awk  '{print $1}' | sort -nr  |  uniq -c
```



### (3）helloworld

```bash
#!/bin/bash

function example {
echo "Hello world!"
}
example
```



### （4）打印 tomcat 的pid



```bash
#!/bin/sh`

v1="Hello"
v2="world"
v3=${v1}${v2}
echo $v3

pidlist=`ps -ef|grep apache-tomcat-7.0.75|grep -v "grep"|awk '{print $2}'`
echo $pidlist
echo "tomcat Id list :$pidlist"  //显示pid
```



### （5）脚本编写 剪刀 、 石头、布游戏



```bash
#!/bin/bash

game=(石头 剪刀 布)
num=$[RANDOM%3]
computer=${game[$sum]}

echo "请根据下列提示选择您的出拳手势"
echo " 1. 石头"
echo " 2. 剪刀"
echo " 3. 布 "

read -p "请选择 1-3 ：" person
case $person in
1)
  if [ $num -eq 0 ]
  then 
    echo "平局"
    elif [ $num -eq 1 ]
    then
      echo "你赢"
    else 
      echo "计算机赢"
fi;;
2)
 if [ $num -eq 0 ]
 then
    echo "计算机赢"
    elif [ $num -eq 1 ] 
    then
     echo "平局"
    else 
      echo "你赢"
fi;;
3)
 if [ $num -eq 0 ]
 then  
   echo "你赢"
   elif [ $num -eq 1 ]
   then 
     echo "计算机赢"
   else 
      echo "平局"
fi;;
*)
  echo "必须输入1-3 的数字"
esac
```



### （6）九九乘法表



```bash
#!/bin/bash

for i in `seq 9`
do 
 for j in `seq $i`
 do 
 echo -n "$j*$i=$[i*j] "
 done
    echo
done
```



### （7）脚本用源码来安装 memcached 服务器



```bash
#!/bin/bash
# 一键部署 memcached （学院官网：www.aliangedu.cn）

# 脚本用源码来安装 memcached 服务器
# 注意:如果软件的下载链接过期了,请更新 memcached 的下载链接
wget http://www.memcached.org/files/memcached-1.5.1.tar.gz
yum -y install gcc
tar -xf  memcached‐1.5.1.tar.gz
cd memcached‐1.5.1
./configure
make
make install
```



### （8）检测本机当前用户是否为超级管理员



```bash
#!/bin/bash

# 检测本机当前用户是否为超级管理员,如果是管理员,则使用 yum 安装 vsftpd,如果不
# 是,则提示您非管理员(使用字串对比版本) 
if [ $USER == "root" ] 
then 
 yum -y install vsftpd
else 
 echo "您不是管理员，没有权限安装软件"
fi
```



### （9）if 运算表达式



```bash
#!/bin/bash -xv

if [ $1 -eq 2 ] ;then
 echo "wo ai wenmin"
elif [ $1 -eq 3 ] ;then 
 echo "wo ai wenxing "
elif [ $1 -eq 4 ] ;then 
 echo "wo de xin "
elif [ $1 -eq 5 ] ;then
 echo "wo de ai "
fi
```



### （10）脚本 杀掉 tomcat 进程并重新启动



```bash
#!/bin/bash

#kill tomcat pid

pidlist=`ps -ef|grep apache-tomcat-7.0.75|grep -v "grep"|awk '{print $2}'`  #找到tomcat的PID号

echo "tomcat Id list :$pidlist"  //显示pid

kill -9 $pidlist  #杀掉改进程

echo "KILL $pidlist:" //提示进程以及被杀掉

echo "service stop success"

echo "start tomcat"

cd /opt/apache-tomcat-7.0.75

pwd 

rm -rf work/*

cd bin

./startup.sh #;tail -f ../logs/catalina.out
```



### （11）打印国际象棋棋盘



```bash
#!/bin/bash
# 打印国际象棋棋盘（学院官网：www.aliangedu.cn）
# 设置两个变量,i 和 j,一个代表行,一个代表列,国际象棋为 8*8 棋盘
# i=1 是代表准备打印第一行棋盘,第 1 行棋盘有灰色和蓝色间隔输出,总共为 8 列
# i=1,j=1 代表第 1 行的第 1 列;i=2,j=3 代表第 2 行的第 3 列
# 棋盘的规律是 i+j 如果是偶数,就打印蓝色色块,如果是奇数就打印灰色色块
# 使用 echo ‐ne 打印色块,并且打印完成色块后不自动换行,在同一行继续输出其他色块
for i in {1..8}
do
   for j in {1..8}
   do
    sum=$[i+j]
  if [  $[sum%2] -eq 0 ];then
    echo -ne "\033[46m  \033[0m"
  else
   echo -ne "\033[47m  \033[0m"
  fi
   done
   echo
done
```



### （12）统计当前 Linux 系统中可以登录计算机的账户有多少个



```bash
#!/bin/bash

# 统计当前 Linux 系统中可以登录计算机的账户有多少个
#方法 1:
grep "bash$" /etc/passwd | wc -l
#方法 2：
awk -f : '/bash$/{x++}end{print x}' /etc/passwd
```



### （13）备份 MySQL 表数据



```bash
#!/bin/sh

source /etc/profile
dbName=mysql
tableName=db
echo [`date +'%Y-%m-%d %H:%M:%S'`]' start loading data...'
mysql -uroot -proot -P3306 ${dbName} -e "LOAD DATA LOCAL INFILE '# /home/wenmin/wenxing.txt' INTO TABLE ${tableName} FIELDS TERMINATED BY ';'"
echo [`date +'%Y-%m-%d %H:%M:%S'`]' end loading data...'
exit
EOF
```



### （14）使用死循环实时显示 eth0 网卡发送的数据包流量



```bash
#!/bin/bash

# 使用死循环实时显示 eth0 网卡发送的数据包流量 

while :
do 
 echo '本地网卡 ens33 流量信息如下：'
 ifconfig ens33 | grep "RX pack" | awk '{print $5}'
     ifconfig ens33 | grep "TX pack" | awk '{print $5}'
 sleep 1
done
```



### （15）编写脚本测试 192.168.4.0/24 整个网段中哪些主机处于开机状态，哪些主机处于关机



```bash
#!/bin/bash

# 编写脚本测试 192.168.4.0/24 整个网段中哪些主机处于开机状态,哪些主机处于关机
# 状态(for 版本)
for i in {1..254}
do 
 # 每隔0.3秒ping一次，一共ping2次，并以1毫秒为单位设置ping的超时时间
 ping -c 2 -i 0.3 -W 1 192.168.1.$i &>/dev/null
     if [ $? -eq 0 ];then
 echo "192.168.1.$i is up"
 else 
 echo "192.168.1.$i is down"
 fi
done
```



### （16）编写脚本：提示用户输入用户名和密码,脚本自动创建相应的账户及配置密码。如果用户



```bash
#!/bin/bash
# 编写脚本:提示用户输入用户名和密码,脚本自动创建相应的账户及配置密码。如果用户
# 不输入账户名,则提示必须输入账户名并退出脚本;如果用户不输入密码,则统一使用默
# 认的 123456 作为默认密码。

read -p "请输入用户名：" user
#使用‐z 可以判断一个变量是否为空,如果为空,提示用户必须输入账户名,并退出脚本,退出码为 2
#没有输入用户名脚本退出后,使用$?查看的返回码为 2
if [ -z $user ]; then
 echo " 您不需要输入账户名" 
 exit 2
fi 
#使用 stty ‐echo 关闭 shell 的回显功能
#使用 stty  echo 打开 shell 的回显功能
stty -echo 
read -p "请输入密码：" pass
stty echo 
pass=${pass:-123456}
useradd "$user"
echo "$pass" | passwd --stdin "$user"
```



### （17）使用脚本对输入的三个整数进行排序



```bash
#!/bin/bash

# 依次提示用户输入 3 个整数,脚本根据数字大小依次排序输出 3 个数字
read -p " 请输入一个整数：" num1
read -p " 请输入一个整数：" num2
read -p " 请输入一个整数:  " num3

# 不管谁大谁小,最后都打印 echo "$num1,$num2,$num3"
# num1 中永远存最小的值,num2 中永远存中间值,num3 永远存最大值
# 如果输入的不是这样的顺序,则改变数的存储顺序,如:可以将 num1 和 num2 的值对调
tmp=0
# 如果 num1 大于 num2,就把 num1 和和 num2 的值对调,确保 num1 变量中存的是最小值
if [ $num1 -gt $num2 ];then
 tmp=$num1
 num1=$num2
 num2=tmp
fi
# 如果 num1 大于 num3,就把 num1 和 num3 对调,确保 num1 变量中存的是最小值
if [ $num1 -gt $num3 ];then
 tmp=$num1
 num1=$num3
 num3=$tmp
fi
# 如果 num2 大于 num3,就把 num2 和 num3 对调,确保 num2 变量中存的是最小值
if [ $num2 -gt $num3 ];then
 tmp=$num2
 num2=$num3
 num3=$tmp
fi
echo "排序后数据（从小到大）为：$num1,$num2,$num3"
```



### （18）根据计算机当前时间，返回问候语，可以将该脚本设置为开机启动

```bash
#!/bin/bash
# 根据计算机当前时间,返回问候语,可以将该脚本设置为开机启动 
（学院官网：www.aliangedu.cn）# 00‐12 点为早晨,12‐18 点为下午,18‐24 点为晚上
# 使用 date 命令获取时间后,if 判断时间的区间,确定问候语内容
tm=$(date +%H)
if [ $tm -le 12 ];then
 msg="Good Morning $USER"
elif [ $tm -gt 12 -a $tm -le 18 ];then
   msg="Good Afternoon $USER"
else
   msg="Good Night $USER"
fi
echo "当前时间是:$(date +"%Y‐%m‐%d %H:%M:%S")"
echo -e "\033[34m$msg\033[0m"
```



### （19）将 I lov cls 写入到 txt 文件中

```bash
#!/bin/bash

cd /home/wenmin/
touch wenxing.txt
echo "I lov cls" >>wenxing.txt
```



### （20）脚本编写 for 循环判断



```bash
#!/bin/bash

s=0;
for((i=1;i<100;i++))
do 
 s=$[$s+$i]
done 

echo $s

r=0;
a=0;
b=0;
for((x=1;x<9;x++))
do 
 a=$[$a+$x] 
echo $x
done
for((y=1;y<9;y++))
do 
 b=$[$b+$y]
echo $y

done

echo $r=$[$a+$b]
```



### （21）脚本编写 for 循环判断

```bash
#!/bin/bash

for i in "$*"
do 
 echo "wenmin xihuan $i"
done

for j in "$@"
do 
 echo "wenmin xihuan $j"
done
```



### （22）脚本 每周 5 使用 tar 命令备份/var/log 下的所有日志文件



```bash
#!/bin/bash
# 每周 5 使用 tar 命令备份/var/log 下的所有日志文件
# vim  /root/logbak.sh
# 编写备份脚本,备份后的文件名包含日期标签,防止后面的备份将前面的备份数据覆盖
# 注意 date 命令需要使用反引号括起来,反引号在键盘<tab>键上面

tar -czf log-`date +%Y%m%d`.tar.gz /var/log 

# crontab -e #编写计划任务，执行备份脚本
00 03 * * 5 /home/wenmin/datas/logbak.sh
```



### （23）脚本编写 求和 函数运算 function xx()

```bash
#!/bin/bash

function sum()
{
 s=0;
 s=$[$1+$2]
 echo $s
}
read -p "input your parameter " p1
read -p "input your parameter " p2

sum $p1 $p2

function multi()
{
 r=0;
 r=$[$1/$2]
 echo $r
}
read -p "input your parameter " x1
read -p "input your parameter " x2

multi $x1 $x2

v1=1
v2=2
let v3=$v1+$v2
echo $v3
```



### （24）脚本编写 case — esac 分支结构表达式

```bash
#!/bin/bash 

case $1 in 
1) 
 echo "wenmin "
;;
2)
 echo "wenxing "
;; 
3)  
 echo "wemchang "
;;
4) 
 echo "yijun"
;;
5)
 echo "sinian"
;;
6)  
 echo "sikeng"
;;
7) 
 echo "yanna"
;;
*)
 echo "danlian"
;; 
esac
```



### （25）定义要监控的页面地址，对 tomcat 状态进行重启或维护



```bash
#!/bin/sh  
# function:自动监控tomcat进程，挂了就执行重启操作  
# author:huanghong  
# DEFINE  

# 获取tomcat PPID  
TomcatID=$(ps -ef |grep tomcat |grep -w 'apache-tomcat-7.0.75'|grep -v 'grep'|awk '{print $2}')  

# tomcat_startup  
StartTomcat=/opt/apache-tomcat-7.0.75/bin/startup.sh  


#TomcatCache=/usr/apache-tomcat-5.5.23/work  

# 定义要监控的页面地址  
WebUrl=http://192.168.254.118:8080/

# 日志输出  
GetPageInfo=/dev/null  
TomcatMonitorLog=/tmp/TomcatMonitor.log  

Monitor()  
  {  
   echo "[info]开始监控tomcat...[$(date +'%F %H:%M:%S')]"  
   if [ $TomcatID ]
 then  
      echo "[info]tomcat进程ID为:$TomcatID."  
      # 获取返回状态码  
      TomcatServiceCode=$(curl -s -o $GetPageInfo -m 10 --connect-timeout 10 $WebUrl -w %{http_code})  
      if [ $TomcatServiceCode -eq 200 ];then  
          echo "[info]返回码为$TomcatServiceCode,tomcat启动成功,页面正常."  
      else  
          echo "[error]访问出错，状态码为$TomcatServiceCode,错误日志已输出到$GetPageInfo"  
          echo "[error]开始重启tomcat"  
          kill -9 $TomcatID  # 杀掉原tomcat进程  
          sleep 3  
          #rm -rf $TomcatCache # 清理tomcat缓存  
          $StartTomcat  
      fi  
      else  
      echo "[error]进程不存在!tomcat自动重启..."  
      echo "[info]$StartTomcat,请稍候......"  
      #rm -rf $TomcatCache  
      $StartTomcat  
    fi  
    echo "------------------------------"  
   }  
   Monitor>>$TomcatMonitorLog
```



### （26）通过位置变量创建 Linux 系统账户及密码



```bash
#!/bin/bash

# 通过位置变量创建Linux 系统账户及密码

# $1 是执行脚本的第一个参数，$2  是执行脚本的第二个参数

useradd "$1"
echo "$2" | passwd --stdin "$1"
```



### （27）对变量的传入与获取个数及打印

```bash
#!/bin/bash
echo "$0 $1 $2 $3"  // 传入三个参数
echo $#    //获取传入参数的数量
echo $@    //打印获取传入参数
echo $*    //打印获取传入参数
```



### （28）实时监控本机内存和硬盘剩余空间，剩余内存小于500M、根分区剩余空间小于1000M时，发送报警邮件给root管理员



```bash
#!/bin/bash

# 实时监控本机内存和硬盘剩余空间,剩余内存小于500M、根分区剩余空间小于1000M时,发送报警邮件给root管理员

# 提取根分区剩余空间
disk_size=$(df / | awk '/\//{print $4}')

# 提取内存剩余空空间
mem_size=$(free | awk '/Mem/{print $4}')
while :
do 
# 注意内存和磁盘提取的空间大小都是以 Kb 为单位
if  [  $disk_size -le 512000 -a $mem_size -le 1024000  ]
then
    mail  ‐s  "Warning"  root  <<EOF
 Insufficient resources,资源不足
EOF
fi
done
```



### （29）检查指定目录下是否存在 对应 文件



```bash
#!/bin/bash

if [ -f /home/wenmin/datas ]
then 
echo "File exists"
fi
```



### （30）脚本定义while循环语句



```bash
#!/bin/bash

if [ -f /home/wenmin/datas ]
then 
echo "File exists"
fi

[root@rich datas]# cat while.sh 
#!/bin/bash

s=0
i=1
while [ $i -le 100 ]
do
        s=$[$s + $i]
        i=$[$i + 1]
done

echo $s
echo $i
```



### （31）一键部署 LNMP（RPM 包版本）

```bash
#!/bin/bash 

# 一键部署 LNMP(RPM 包版本)
# 使用 yum 安装部署 LNMP,需要提前配置好 yum 源,否则该脚本会失败
# 本脚本使用于 centos7.2 或 RHEL7.2
yum -y install httpd
yum -y install mariadb mariadb-devel mariadb-server
yum -y install php php-mysql

systemctl start httpd mariadb
systemctl enable httpd mariadb
```



### （32）读取控制台传入参数



```bash
#!/bin/bash
read -t 7 -p "input your name " NAME
echo $NAME

read -t 11 -p "input you age " AGE
echo $AGE

read -t 15 -p "input your friend " FRIEND
echo $FRIEND

read -t 16 -p "input your love " LOVE
echo $LOVE
```



### （33）脚本实现 复制



```bash
#!/bin/bash

cp $1 $2
```



### （34）脚本实现文件存在与否的判断



```bash
#!/bin/bash

if [ -f file.txt ];then
 echo "文件存在"
else 
 echo "文件不存在"
fi
```

