## 2.变量简介
　　Linux是一个多用户的操作系统。每个用户登录系统后，都会有一个专用的运行环境。通常每个用户默认的环境都是相同的，这个默认环境实际上就是一组环境变量的定义。用户可以对自己的运行环境进行定制，其方法就是修改相应的系统环境变量。
　　
## 3.定制环境变量

　　环境变量是和Shell紧密相关的，用户登录系统后就启动了一个Shell。
　　对于Linux来说一般是bash，但也可以重新设定或切换到其它的Shell（使用chsh命令）。
　　根据发行版本的情况，bash有两个基本的系统级配置文件：
1
/etc/bashrc和/etc/profile。

(2)在profile文件末尾加入：
JAVA_HOME=/usr/share/jdk1.5.0_05
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH

这些配置文件包含两组不同的变量：

shell变量和环境变量。

前者只是在特定的shell中固定（如bash），后者在不同shell中固定。很明显，shell变量是局部的，而环境变量是全局的。环境变量是通过Shell命令来设置的，设置好的环境变量又可以被所有当前用户所运行的程序所使用。对于bash这个Shell程序来说，可以通过变量名来访问相应的环境变量，通过export来设置环境变量。

注：Linux的环境变量名称一般使用大写字母

4.环境变量设置实例

 

1.使用命令echo显示环境变量

本例使用echo显示常见的变量HOME

1
2
3
$ echo $HOME 
 
/home/kevin
　　

2.设置一个新的环境变量

1
2
3
4
5
$ export MYNAME=”my name is kevin”
 
$ echo $ MYNAME
 
my name is Kevin
　　

3.修改已存在的环境变量

接上个示例

1
2
3
4
5
$ MYNAME=”change name to jack”
 
$ echo $MYNAME
 
change name to jack
　　

4.使用env命令显示所有的环境变量

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
$ env
 
HOSTNAME=localhost.localdomain
 
SHELL=/bin/bash
 
TERM=xterm
 
HISTSIZE=1000
 
SSH_CLIENT=192.168.136.151 1740 22
 
QTDIR=/usr/lib/qt-3.1
 
SSH_TTY=/dev/pts/0
 
……
　　

5.使用set命令显示所有本地定义的Shell变量

1
2
3
4
5
6
7
$ set
 
BASH=/bin/bash
 
BASH_ENV=/root/.bashrc
 
……
　　

6.使用unset命令来清除环境变量

1
2
3
4
5
6
7
8
9
$ export TEMP_KEVIN=”kevin”     #增加一个环境变量TEMP_KEVIN
 
$ env | grep TEMP_KEVIN          #查看环境变量TEMP_KEVIN是否生效（存在即生效）
 
TEMP_KEVIN=kevin #证明环境变量TEMP_KEVIN已经存在
 
$ unset TEMP_KEVIN            #删除环境变量TEMP_KEVIN
 
$ env | grep TEMP_KEVIN       #查看环境变量TEMP_KEVIN是否被删除，没有输出显示，证明TEMP_KEVIN被清除了。
　　

7.使用readonly命令设置只读变量

注：如果使用了readonly命令的话，变量就不可以被修改或清除了。

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
$ export TEMP_KEVIN ="kevin"      #增加一个环境变量TEMP_KEVIN
 
$ readonly TEMP_KEVIN                  #将环境变量TEMP_KEVIN设为只读
 
$ env | grep TEMP_KEVIN          #查看环境变量TEMP_KEVIN是否生效
 
TEMP_KEVIN=kevin        #证明环境变量TEMP_KEVIN已经存在
 
$ unset TEMP_KEVIN          #会提示此变量只读不能被删除
 
-bash: unset: TEMP_KEVIN: cannot unset: readonly variable
 
$ TEMP_KEVIN ="tom"        #修改变量值为tom会提示此变量只读不能被修改
 
-bash: TEMP_KEVIN: readonly variable
　　

8.通过修改环境变量定义文件来修改环境变量。

需要注意的是，一般情况下，仅修改普通用户环境变量配置文件，避免修改根用户的环境定义文件，因为那样可能会造成潜在的危险。

1
2
3
4
5
$ cd ~                                  #到用户根目录下
 
$ ls -a                                 #查看所有文件，包含隐藏的文件
 
$ vi .bash_profile                #修改用户环境变量文件
　　

例如：

编辑你的PATH声明，其格式为：

1
PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH N>
　　

你可以自己加上指定的路径，中间用冒号隔开。

环境变量更改后，在用户下次登陆时生效。

如果想立刻生效，则可执行下面的语句：

1
$source .bash_profile
　　

需要注意的是，最好不要把当前路径”./”放到PATH里，这样可能会受到意想不到的攻击。

完成后，可以通过echoPATH查看当前的搜索路径。这样定制后，就可以避免频繁的启动位于shell搜索的路径之外的程序了。

 

5.学习总结

 

1.Linux的变量种类

按变量的生存周期来划分，Linux变量可分为两类

1
2
3
1.     永久的：需要修改配置文件，变量永久生效
 
2.     临时的：使用export命令行声明即可，变量在关闭shell时失效。
　　

2.设置变量的三种方法

 

2.1. 在/etc/profile文件中添加变量【对所有用户生效（永久的）】

用VI在文件/etc/profile文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。

例如：编辑/etc/profile文件，添加CLASSPATH变量

1
2
3
# vi /etc/profile
 
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
注：修改文件后要想马上生效还要运行# source /etc/profile不然只能在下次重进此用户时生效

 

2.2     在用户目录下的.bash_profile文件中增加变量【对单一用户生效（永久的）】

用VI在用户目录下的.bash_profile文件中增加变量，改变量仅会对当前用户有效，并且是“永久的”。

例如：编辑guok用户目录（/home/guok）下的.bash_profile

1
2
3
4
5
$ vi /home/guok/.bash.profile
 
添加如下内容：
 
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
注：修改文件后要想马上生效还要运行$ source /home/guok/.bash_profile不然只能在下次重进此用户时生效

 

2.3     直接运行export命令定义变量【只对当前shell（BASH）有效（临时的）】

在shell的命令行下直接使用[export变量名=变量值]定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。
