Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。每个用户账号都拥有一个唯一的用户名和各自的口令。用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。 

> 用户账号的添加、删除与修改

添加用户语法：useradd 选项 用户名

参数说明：

-  选项: 

  - -c comment 指定一段注释性描述。
  - -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
  - -g 用户组 指定用户所属的用户组。
  - -G 用户组，用户组指定用户所属的附加组。
  - -s Shell文件 指定用户的登录Shell。
  - -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

- 用户名: 

  指定新账号的登录名。



```bash
[root@VM-20-7-centos /]# useradd weick
[root@VM-20-7-centos /]# cd home
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  weick  www
[root@VM-20-7-centos home]# ls -l
total 16
drwx------ 5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x 2 root       root       4096 Feb 23 09:47 mydoc
drwx------ 2 weick      weick      4096 Feb 23 22:28 weick
drwxr-xr-x 2 root       root       4096 Jan 14 11:27 www
[root@VM-20-7-centos home]# 
```

删除用户语法：userdel 选项 用户名

常用的选项是 -r，它的作用是把用户的主目录一起删除。

```bash
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  uuu  weick  www
[root@VM-20-7-centos home]# userdel -r uuu
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  weick  www

```

此命令删除用户 uuu 在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录，同时删除用户的主目录。



修改账号语法：usermod 选项 用户名

常用的选项包括`-c, -d, -m, -g, -G, -s, -u以及-o等`，这些选项的意义与`useradd`命令中的选项一样，可以为用户指定新的资源值。 另外，有些系统可以使用选项：-l 新用户名这个选项指定一个新的账号，即将原来的用户名改为新的用户名。 

```bash
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  weick  www  zhangsan
[root@VM-20-7-centos home]# usermod -l zhangsan lisi
usermod: user 'lisi' does not exist
[root@VM-20-7-centos home]# usermod -l lisi zhangsan
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  weick  www  zhangsan
[root@VM-20-7-centos home]# cd zhangsan
[root@VM-20-7-centos zhangsan]# ls
```

zhangsan 修改 lisi 成功，还是在home里面，被隐藏了！！！

```
[root@VM-20-7-centos zhangsan]# cd ..
[root@VM-20-7-centos home]# ls -al
total 28
drwxr-xr-x.  7 root       root       4096 Feb 23 22:37 .
dr-xr-xr-x. 20 root       root       4096 Feb 23 22:43 ..
drwx------   5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x   2 root       root       4096 Feb 23 09:47 mydoc
drwx------   2 weick      weick      4096 Feb 23 22:28 weick
drwxr-xr-x   2 root       root       4096 Jan 14 11:27 www
drwx------   2 lisi       zhangsan   4096 Feb 23 22:37 zhangsan
[root@VM-20-7-centos home]# passwd zhangsan
passwd: Unknown user name 'zhangsan'.
[root@VM-20-7-centos home]# passwd lisi
Changing password for user lisi.
New password: 
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@VM-20-7-centos home]# su lisi
[lisi@VM-20-7-centos home]$ ls
lighthouse  mydoc  weick  www  zhangsan

```



```bash
[lisi@VM-20-7-centos ~]$ ls
[lisi@VM-20-7-centos ~]$ cd .. //这里返回home看出是在home目录下
[lisi@VM-20-7-centos home]$ ls
lighthouse  mydoc  weick  www  zhangsan
[lisi@VM-20-7-centos home]$ ls -al
total 28
drwxr-xr-x.  7 root       root       4096 Feb 23 22:37 .
dr-xr-xr-x. 20 root       root       4096 Feb 23 22:53 ..
drwx------   5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x   2 root       root       4096 Feb 23 09:47 mydoc
drwx------   2 weick      weick      4096 Feb 23 22:28 weick
drwxr-xr-x   2 root       root       4096 Jan 14 11:27 www
drwx------   4 lisi       zhangsan   4096 Feb 23 22:45 zhangsan

```



> 用户口令管理

用户管理的一项重要内容是用户口令的管理。用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令。 指定和修改用户口令的Shell命令是`passwd`。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。命令的格式为：`passwd` 选项 用户名

 可使用的选项： 

- -l 锁定口令，即禁用账号。
- -u 口令解锁。
- -d 使账号无口令。
- -f 强迫用户下次登录时修改口令。





> 遇到问题

当想删除某个用户的时候，出现 user xxx is currently used by process xxx，可能的原因是创建用户user1之后，使用su命令切换到user1用户下，之后又想删除user1用户，使用su root切换到root用户下，使用userdel user1。出现上述情况的根本原因在于切换回root用户之后，user1还被某个进程占用。

```
[root@VM-20-7-centos home]# su lisi
[lisi@VM-20-7-centos home]$ ls
lighthouse  mydoc  weick  www  zhangsan
[lisi@VM-20-7-centos home]$ userdel -r lisi
userdel: user lisi is currently used by process 18658
[lisi@VM-20-7-centos home]$ su root
Password: 
[root@VM-20-7-centos home]# userdel -r lisi
userdel: user lisi is currently used by process 18658

```

解决方案：ctrl+d（退出当前用户）第一次使用ctrl+d退出root用户，回到user1用户；第二次使用ctrl+d退出user1用户，此时会返回到root用户（再按ctrl+d退出登陆连接），此时使用userdel user1正常删除。

```bash
[root@VM-20-7-centos home]# userdel -r lisi
userdel: user lisi is currently used by process 18658
[root@VM-20-7-centos home]# exit
[lisi@VM-20-7-centos home]$ exit
[root@VM-20-7-centos home]# userdel -r lisi
[root@VM-20-7-centos home]# ls
lighthouse  mydoc  weick  www //删除了更名后的李四，张三被删除了！

```

也可以把对应的进程杀了再删

```bash
ps -u username | awk '{print $1}' | grep -vi pid | xargs kill -9 && deluser username
```

或暴力删除

```bash
userdel -r -f XXXname
```

