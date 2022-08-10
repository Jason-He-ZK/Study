### Linux

##### 目录结构

```plaintext
1) bin --> usr/bin : 这个目录存放最经常使用的命令
2) boot : 这个目录存放启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件
3) dev: dev 是 Device( 设备 的缩写 , 该目录下存放的是 Linux 的外部设备， Linux 中的设备也是以文件的形式存在
**4) etc : 这个目录存放所有的系统管理所需要的配置文件**
5) home ：用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名以用户的账号命名
6) lib user/lib: 这个目录存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件，几乎所有的应用程序都需要用到这些共享库。
7) mnt : 系统提供该目录是为了让用户临时挂载别的文件系统，我们可以将光驱挂载在/ 上，然后进入该目录就可以查看光驱里的内容
8) opt: 这是给 linux 额外安装软件所存放的目录。比如你安装一个 Oracle 数据库则就可以放到这个目录下，默认为空。
9) root : 该目录为系统管理员目录， ro ot 是具有超级权限的用户
10) tmp: 这个目录是用来存放一些临时文件的。
**11) usr: 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与windows 下的 program files 目录。**
12) var : 这个目录存放着在不断扩充着的东西，我们习惯将那些经常被修改的文件存放在该目录下，比如运行的各种日志文件。
```

##### 目录、文件、压缩

**目录与位置**
pwd，cd
ll（ls -l），ls
**创建目录**
mkdir 目录名
**删除文件或目录**
rm -rf 目录名或者文件名
r：递归，f：不提示确认
**复制与剪切**
cp 被复制的文件名 新文件名（文件名可带路径）
mv 旧文件 新文件
r：递归，f：不提示确认
**查看文件**
cat 文件路径 （全部内容）
more 文件路径 （分页查看）
head 文件路径 -n 数字 （默认是 10 行）
tail 文件路径 -n 数字 （默认是 10 行）

**文件内搜索**

```
grep [参数] 搜索的字符串内容 文件名1 文件名2 ...，
可以使用正则表达式

② 搜索文本 ”java” 不区分大小写： grep -i java aa.txt
③ 搜索的文本中有空格，使用引号括起来：grep “java is” aa.txt
④ 搜索整个单词，是其他字符串的一部分的不符合条件：grep -w java aa.txt
⑦ 使用管道：cat aa.txt | grep java
```

**tar**

```
tar 参数 要 压缩/解压 的 文件/目录

z：使用压缩 ，生成的文件名是 xxx.tar.gz，这是 linux 中常用的压缩格式。
c：创建压缩文档
v：显示压缩，解压过程中处理的文件名
f：指定归档文件名指定归档文件名, tar, tar参数后面是归档文件名参数后面是归档文件名
x：从归档文件中释放文件，就是解压。从归档文件中释放文件，就是解压。
t：列出归档文件内容，查看文件内容列出归档文件内容，查看文件内容
C：解压到指定目录解压到指定目录，使用方式，使用方式 --C 目录目录 ， C 是大写的。
```

**压缩**

```
tar -zvcf 归档文件名 要归档文件列表/目录
tar -zcvf txtfile.tar.gz aa.txt
tar -zcvf txt.tar.gz aa.txt test.txt
tar -zcvf txt2.tar.gz *.txt
tar -zcvf file.tar.gz mytest
```

查看

```
tar -tf 归档文件名
```

解压

```
tar -zxvf 已归档的文件名
```

##### 系统、网络

```
date
clear
```

切换用户

```
su 用户名
```

重启，关机

```
reboot
shutdown -h now
```

查看系统进程

```
ps -ef
e：显示当前所有进程
f：显示 UID,PPID,C 与 STIME 栏位 信息

UID：拥有改程序的用户
PID：程序的进程 id
PPID：父进程的 id
C：CPU 使用的资源百分比
STIME：系统启动时间
TTY：登录系统的终端位置（客户端的标识
TIME：使用掉的 CPU 时间
CMD：进程是有哪些程序启动的
```

结束进程

```
kill 进程pid
-9 强制结束
```

查看 ip 信息

```
ifconfig
```

测试网络连通

```
ping
```

curl ：使用 url 访问网络的文件传输工具。

```
curl www.baidu.com
常用来①： 测试网络访问；③：模拟用户访问
```

下载

```
wget 资源的地址
```

##### 权限

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038479174-9cd18384-30fc-4fee-9f89-55c8b2f5b2dd.png#clientId=u3afd84e8-66b7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=124&id=ueadf58cd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=248&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52958&status=done&style=none&taskId=ub7375f93-d436-4572-98de-ea7d39f4a7d&title=&width=391.5)；

```
-：表示文件
l：软链接 文件（ windows 快捷方式）
d：目录
c：字符设备文件 一次传输一个字节的设备被称为字符设备。 例如键盘，鼠标
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038482716-4e2511f4-174b-4ef6-a022-444d68f14b6a.png#clientId=u3afd84e8-66b7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=83&id=ua9bf530a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=165&originWidth=324&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30224&status=done&style=none&taskId=u34ade78a-4a35-43df-bba9-70b5f95f511&title=&width=162)；

```
linux 权限机制采用 UGO 模式。其中 u(user) 表示所属用户、 g(group) 表示所属组、 o(other) 表示除了所属用户、所属组之外的情况。
r--read 读权限 4
w--write 写权限 2
x--execute 执行权限 1
rwx= 4 + 2 + 1 = 7
常见 644 、 755 、 777 三种权限
```

修改文件权限

```
chmod UGO权限 文件/目录
chmod 606 aa.txt
```

修改文件拥有者

```
chown 新的拥有者用户 被修改的文件
```

##### 管道和重定向

重定向输出，覆盖  >，追加  >>

```
echo write some > t1.txt
echo “hello new word” >> t1.txt
```

管道

```
管道就是用“|”连接两个命令，以前面一个命令的输出作为后面命令的 输入 用于把管道左边的输出作为右边的输入。
语法：命令 1 命令 2 | 命令 n
例如：echo “hello linux” | wc
```

##### vi 和 vim 编辑器

启动 vi 编辑器

```
vi 文件名（存在就打开，没有就创建）
```

```
命令模式：按 Esc 键，进入命令模式，命令模式下无法编辑（ :wq 保存退出    :q! 不保存退出)
编辑模式：按 a 或者 i 或者 o 字母键，进入编辑模式（底部会出现 insert）
```

```
dd ：删除光标所在行
yy ：复制光标所在行到缓冲区
p ：粘贴缓冲区中的内容
gg ：光标回到文件第一行
GG ：光标回到文件最后一行
^ ：光标移动至当前行的行首
$ ：光标移动至当前行的行尾
/asdf：搜索asdf，按 n 键查找下一个
```

##### 安装软件命令

查找，安装，删除

```
yum search 安装包名称中的部分关键字
yum install 安装包名称
yum remove 安装包名称
```

列出所有已安装的软件包

```
yum list installed
```

清除已安装 软件包 的下载文件

```
yum clean all（yum 命令下载的安装包都放在/var/cache/yum 目录）
```

**防火墙**
systemctl status firewalld
systemctl stop firewalld
systemctl start firewalld
禁用启用
systemctl disable firewalld
systemctl enable firewalld
