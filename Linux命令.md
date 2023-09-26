# 基本

```shell
./表示当前目录
 /表示根目录
..表示上级目录
~ 表示根目录，即登录用户的目录，会根据登录账号不同而改变，例如：
root@JaniusBisecond-LAPTOP:/home/janius # cd ~     其中@前面表示登录用户
root@JaniusBisecond-LAPTOP:~ # pwd
/root
root@JaniusBisecond-LAPTOP:~ # su janius
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

janius@JaniusBisecond-LAPTOP:/root$ cd ~
janius@JaniusBisecond-LAPTOP:~$ pwd
/home/janius
```

# 用户相关

切换到另外一个用户

```shell
su - username
```

进入root

```shell
sudo su
```

退出root

```shell
exit
```

创建用户

```shell
useradd -m <用户名> #-m自动建立用户的登入目录
passwd <用户名>     #设置密码
```

设置用户权限

```shell
vim /etc/sudoers
```

![image-20221004171330324](https://github.com/JaniusBisecond/Janius-note/blob/master/images/Linux命令/image-20221004171330324.png)

删除用户

```shell
userdel -r <用户名>
```

设置默认登录用户(cmd中),并在Windows Terminal中更改启动目录为 //wsl$/Ubuntu-22.04/home/janius

```shell
D:\Ubuntu2204LTS>ubuntu2204.exe config --default-user janius
```

# 环境变量

每个用户目录下都有`~/.bashrc  ~/.bash_profile  ~/.bash_login`这些文件，当通过shell启动程序时登录用户时，它们会被自动加载。如果是图形化启动这些文件可能就不会加载。

`/etc/profile`该文件的作用域是全局，可以将系统的环境变量存放于此，这样所有用户都能使用。（注意只是作用域是全局，但并非是指生命周期，重启依旧会失效）

如果希望`/etc/profile`中的内容每次都能被加载，可以在各个用户的`~/.bashrc`文件中添加`source /etc/profile`，这样用户登录就自动加载全局配置

```shell
临时环境变量：
export GOROOT=/usr/local/go    添加环境变量(临时，shell关闭时消失)
echo $GOROOT				  查看指定环境变量
env							 显示所有环境变量配置

永久环境变量：
用户级：
vim ~/.bashrc
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
关闭~/.bashrc
source ~/.bashrc
系统级：
sudo vim /etc/profile
....
```

# ssh服务

```shell
ps -e | grep ssh            检测是否启动
/etc/init.d/ssh start       启动服务
sudo /etc/init.d/ssh resart 重启服务

在linux中，etc是一个目录，用于存放系统主要的配置文件，来源于法语“et cetera”；该目录下的文件属性可以让一般用户进行查阅，但是只有root用户有权利进行修改。
“.d”文件表示的是：1、依赖文件，其中d是dependence的意思；2、默认配置文件，其中d是default的意思；3、动态意义的文件，其中d是Dynamic的意思。
```

# 查找命令

```shell
find 文件夹
find -name 文件名
```

# 删除文件

```shell
rm -rf 目录名
```

# 复制一个新的文件夹

```shell
cp -r 源文件夹 新目的文件夹
```

# 移动文件和目录

```shell
mv 目标文件 目标文件 ... 目的位置
```

# 重命名

```shell
mv 旧名字 新名字
```

# 权限相关

```shell
ls -l [-filename] ;查看权限,drwxrwxrwx,r可读权限，w可写权限，x可执行权限 d-user-group-other
chmod ugo+r file1.txt ;给所有人增加读权限
chmod a+r file1.txt	  ;同上
chmod -R a+r *	      ;将目前目录下的所有文件与子目录皆设为任何人可读取 -R参数代表递归，类似于rm -rf
chmod 777 file        ;用数字表示drwxrwxrwx
chmod ug+w,o-w file1.txt file2.txt; 将文件 file1.txt 与 file2.txt 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 
```

# 建立软链接

```shell
ln -s 源文件 目标文件
```

# 查看进程

```shell
ps aux
```

# 杀死进程

```shell
kill -9 <PID>
```

# 修改主机名

```shell
sudo hostnamectl set-hostname <新主机名>
```

# 配色

```shell
echo $PS1 #查看当前配色
#正在使用的配色
PS1='${debian_chroot:+($debian_chroot)}\[\033[36;01m\]\u\[\033[37;01m\]@\[\033[36;01m\]\h\[\033[30;01m\]:\[\033[34;01m\]\w \[\033[36;01m\]\$ \[\033[37;01m\]'
#添加到~/.bashrc永久保存,仅针对当前用户
#添加到/etc/profile，并source /etc/profile可以全部用户通用
```

