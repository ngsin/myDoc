## shell脚本

##### shell脚本注意事项

- shell内容以`#!/bin/bash`开头
- 变量名称尽量使用大写字母，字母间不能使用`-`，但是可以使用`_`

##### 执行命令shell脚本的方法

- `./shellscript.sh`
- `/bin/sh shellscript.sh`

### shell变量

shell为弱类型语言，定义变量不需要声明类型。

可以使用declare指定类型。

shell编程中变量分为：系统变量、环境变量、用户变量。

- 系统变量在对参数判断和命令返回值判断时使用。
- 环境变量只要是在程序运行时需要设置。
- 用户变量又称为局部变量，多使用在脚本内部。

##### shell常见的系统变量：

- `$0`：当前脚本的名称。
- `$n`：当前脚本的第n个参数。
- `$*`：当前脚本的所有参数（不包含程序本身）。
- `$#`：当前脚本的参数个数（不包含程序本书）。
- `$?`：命令或程序执行完后返回的状态，返回0表示执行成功。
- `$$`：程序本身的PID号。

##### shell常见环境变量：

- PATH：命令所示路径，以冒号为分割。
- HOME：打印用户家目录。
- SHELL：显示当前shell类型。
- USER：打印。
- ID：打印当前用户ID。
- PWD：显示当前所在路径。
- TERM：打印当前中断类型。
- HOSTNAME：显示当前主机名。

```shell
 #!/bin/bash
echo $PATH：命令所示路径，以冒号为分割。
echo $HOME：打印用户家目录。
echo $SHELL：显示当前shell类型。
echo $USER：打印。
echo $ID：打印当前用户ID。
echo $PWD：显示当前所在路径。
echo $TERM：打印当前中断类型。
echo $HOSTNAME：显示当前主机名。
```

⭐使用到的三剑客

``` shell
awk '{print $0 "demo"}' file #在文本每一行前添加demo
awk '{print "demo" $0}' file #在文本每一行后添加demo
awk '{print "demo" $0 > "file"}' file #将修改后的文本添加到file文件中，文件名用双引号
```

### if条件语句

```
if(表达式);then
	语句1
else
	语句2
fi
```

```shell
if(表达式);then
	语句1
elif(表达式);
	语句2
elif(表达式);
	语句3
fi
```



##### 案例（比较两个整数大小）

```shell
#!/bin/bash
NUM=100
if(($NUM>4));then
	echo "The $NUM more than 4"
else
	echo "The $NUM less then 4"
fi
```

##### 案例（判断文件是否存在）

```shell
#!/bin/bash
if [ ! -f file1 -a ! -f file2 ];then
touch file1
fi
```

##### 常见判断逻辑运算符

```shell
-f:判断文件是否存在
-d:判断目录是否存在
-eq:等于，整形比较
-ne:不等于，整形比较
-lt:小于，整形比较
-gt:大于，整形比较
-le:小于或等于，整形比较
-ge:大于或等于，整形比较
-a:逻辑表达式and
-o:逻辑表达时or
-z:空字符串
||:单方成立
&&:双方成立
```

##### 案例（判断分数）

```shell
#!/bin/bash
scores=$1
if [[ $scores -eq 100 ]]; then
        echo "very good!";
elif [[ $scores -gt 85 ]]; then
        echo "good!";
elif [[ $scores -gt 60 ]];then
        echo "pass!"
elif [[ $scores -lt 60 ]];then
        echo "no pass!"
fi
```

### if判断括号区别

```shell
(): 用于多个命令组

(()): 整数扩展、运算符、重定义变量值、算数运算比较
[]: bash内部命令，与test相同。正则字符范围、引用数组元素编号，不支持“+”，“-，“*”，“/”数学运算符，逻辑测试使用-a、-o。

[[]]: bash程序语言关键字，不是一个命令，比[]更加通用，不支持“+”，“-，“*”，“/”数学运算符，逻辑测试使用&&、||。

{}： 主要用于命令集合或范围，例如mkdir -p /data/201{7,8}
```

⭐创建多个文件

```shell
touch file{0..5} # 创建file0~file5
```

##### 实战（MySQL数据库备份脚本）

```shell
#!/bin/bash
# auto backup mysql
# By author kevinjohn 2020/11/13
BAKDIR=/data/backup/mysql/`date +%Y-%m-%d`
MYSQLDB=test    #定义数据库变量
MYSQLPW=1       #数据库密码
MYSQLUSR=root   #登录数据库用户
if [ $UID -ne 0 ];then  #UID为0表示root用户
        echo This script must use the root user!!!
        sleep 2
        exit 0
fi
if [ ! -d $BAKDIR ];then
        mkdir -p $BAKDIR
fi
/usr/bin/mysqldump -u $MYSQLUSR -p$MYSQLPW -d $MYSQLDB > $BAKDIR/test_db.sql
echo "The mysql backup successfully"
```

⭐变量赋值=不能留空格

### for循环

```shell
for var in (表达式)
do
	语句1
done
```

##### 案例

```shell
# 循环打印bat官网地址
for website in www.baidu.com www.taobao.com www.qq.com
do
	echo $website
done
```
```shell
#循环打印1~100数字，seq表示列出数据范围
for i in `seq 1 100`
do
	echo "NUM is $1"
done
```
```shell
#for循环求1~100的总和
j=0
for((i=1;i<=100;i++))
do
	j=`expr $i + $j`
done
echo $j
```
```shell
# 批量远程主机执行命令
for i in `seq 100 200`
do
	ssh -l root 192.168.1.$i 'ls /tmp'
done
```

### while 循环

```shell
while (表达式)
do
	语句1
done
```

##### 案例

```shell
#读取文件，打印输出
while read line
do
	echo $line
done < /etc/hosts
```
⭐read指令用于读取行或者读取变量
```shell
#while 循环求1~100的总和
i=0
j=1
while(( i <= 100))
do
        j=`expr $i + $j`
        ((i++))
done
echo $j
```

⭐expr用于运算逻辑工具

```shell
# 判断IP正确性
read -p "please enter ip address, example 192.18.0.11 ip" IPADDR
echo $IPADDR | grep -v "[a-zA-Z]" | grep --color -E "([0-9]{1,3}\.){3}[0-9]{1,3}"
while [ $? -ne 0 ]      # 若没有正确匹配,则继续循环
do
	read -p "please enter ip address, example 192.18.0.11
ip" IPADDR
	echo $IPADDR | grep -v "[a-zA-Z]" | grep --color -E "([0-9]{1,3}\.){3}[0-9]{1,3}"
done
```

⭐grep -v 或 --invert-match : 显示不包含匹配文本的所有行。

⭐read -p 后面显示提示信息, 用于从标准输入读取数值

⭐$? 前面程序执行正确则返回0

```shell
# 每5秒检查/etc/passwd文件是否被改变
FILES="/etc/passwd"
while true
do
    echo "The time is `date +%F-%T`"
    OLD=`md5sum $FILES | cut -d " " -f 1`
    echo $OLD
    sleep 5
    NEW=`md5sum $FILES | cut -d " " -f 1`
    echo $NEW
    if [[ $OLD != $NEW ]]; then
            echo "The $FILES has been modified."
    fi
done
```

⭐`md5sum testfile  > testfiel.md5` 产生md5摘要信息

⭐`md5sum -c testfile.md5` 同一目录下执行该命令，验证文件是否修改

```shell
# 每10秒判断kevinjohn用户使用否登录
USERS="kevinjohn"
while true
    do
        echo "The Time is `date +%F-%T`"
        sleep 10
        NUM=`who | grep "$USERS" | wc -l`
        if [[ $NUM -ge 1 ]];then
            echo "The $USERS is login in system"
        fi
    done
```

### case 选择语句

```shell
case $1 in
    Pattern1)
    语句1
    ;;
    Pattern2)
    语句2
    ;;
    *)
    语句3
    ;;
esac
```

### select 选择语句

用于选择, 常用于菜单的创建

```shell
select i in (表达式)
do
    语句
done
```

##### 案例

```shell
# 打印开源操作系统选择
#!/bin/bash
PS3="What you like most of the open source
system?"
select i in Centos RedHat Ubuntu
do
    echo "Your Select System:"$i
done
```

```shell
# 打印LAMP选择菜单
#!/bin/bash
PS3="please enter your selsec install munu:"
select i in http php mysql quit
do
case $i in
    http)
    echo Test httpd;
    ;;
    php)
    echo Test PHP
    ;;
    mysql)
    echo Test MySQL
    ;;
    quit)
    echo The System exit
    exit
esac
done
```

### shell函数

```shell
function name(){
    command1
    command2
    ...
}
```

##### 案例

```shell
# 创建apache安装函数
#!/bin/bash
# auto install apache
H_FILES=httpd-2.2.31.tar.bz2
H_FILES_DIR=httpd-2.2.31
H_URL=https://mirror.bjtu.edu.cn/apache/httpd/
H_PREFIX=/usr/local/apache2/
function Apache_install()
{
# install httpd web server
if [[ "$1" -eq "1" ]];then
    wget -c $H_URL/$H_FILES && tar -jxvf $H_FILES && cd H_FILES_DIR && ./configure --prefix=$H_PREFIX
    if [ $? -eq 0 ];then
        make && make install
        echo -e "\n\033[32m-----------------------\033[0m"
        echo -e "\n\033[32mThe $H_FILE_DIR Server Install Success!\033[0m"
    else
        echo -e "\n\033[32mThe $H_FILE_DIR Make or Make install ERROR, Please Check......"
    fi
fi
}
Apache_install 1 # 执行函数,传递参数1
```

⭐https://mirror.bjtu.edu.cn/软件源



## 数组编程

##### 数组定义及创建

```shell
JETEST={
    test1
    test2
    test3
}
```

##### 引用数组的方法详解

- echo ${JETEST[@]}：显示该数组所有参数
- echo ${#JETEST[@]}：显示该数组参数个数
- echo ${#JETEST[0]}：显示test1字符长度

- echo ${JETEST[@]:0}：打印数组所有的值
- echo ${JETEST[@]:1}：打印从第二个开始所有的值
- echo ${JETEST[@]:1:2}：打印第二个值与第三个值。

##### 数组替换操作

- JETEST=( [0]=www1[1]=www2[2]=www3)：数组赋值
- exho ${JETEST[@]/test/jfedu}：将属猪值test替换为jfedu
- NEWJFTEST='echo ${JFTEST[@]/test/jfedu}'：将结果赋值新数组

##### 数组删除操作

unset array[0]：删除数组第一个值。

unset array[1]：删除数组第二个值。

unset array：删除整个数组。

##### 案例1：网卡bond绑定脚本 (没看懂有什么用)

```shell
#!/bin/bash
eth_bond()
{
NETWORK=(
    HWADDR=`ifconfig eth0 | egrep "HWaddr | Bcast" | tr"\n' "" | awk '{print $5, $7, $NF}' | sed -e 's/addr://g' -e 's/Mask://g' | awk '{print $1}'`
    IPADDR=`ifconfig eth0 | egrep "HWaddr | Bcast" | tr"\n' "" | awk '{print $5, $7, $NF}' | sed -e 's/addr://g' -e 's/Mask://g' | awk '{print $2}'`
    NETMASK=`ifconfig eth0 | egrep "HWaddr | Bcast" | tr"\n' "" | awk '{print $5, $7, $NF}' | sed -e 's/addr://g' -e 's/Mask://g' | awk '{print $3}'`
    GATEWAY=`route -n | grep "UG" | awk '{print $2}'`
)    
}
cat > ifcfg-bond0 <<EOF
DEVICE=bond0
BOOTPROTO=static
${NETWORK[1]}
${NETWORK[2]}
${NETWORK[3]}
ONBOOT=yes
TYPE=Ethernet
NM_CONTROLLED=no
EOF
```

⭐tr 'A-Z' 'a-z'将输入字符中大写字母替换成小写

⭐<<EOF表示后续的输入作为子命令或子shell的输入参数，直到遇到EOF，再次返回到主shell中

##### 案例2：定义IPv4值 (没看懂有什么用)

```shell
#!/bin/bash
# auto change ip netmask gateway scripts
ETHCONF=/etc/sysconfig/network-script/ifcfg-eth0
HOSTS=/etc/hosts
NETWORK=/etc/sysconfig/network
DIR=/data/back/`date +%Y %m %d`
NETMASK=255.255.255.0
echo "--------------------"
count_ip(){
     count=(`echo $IPADDR | awk -F. '{print $1, $2, $3,$4}'`)
         IP1=${count[0]}
         IP2=${count[1]}
         IP3=${count[2]}
         IP4=${count[3]}
}
```

