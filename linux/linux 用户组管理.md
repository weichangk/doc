每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的更新。

```bash
[root@VM-20-7-centos etc]# cat group
root:x:0:
bin:x:1:
daemon:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mem:x:8:
kmem:x:9:
wheel:x:10:
cdrom:x:11:
mail:x:12:postfix
man:x:15:
dialout:x:18:
floppy:x:19:
games:x:20:
tape:x:33:
video:x:39:
ftp:x:50:
lock:x:54:
audio:x:63:
nobody:x:99:
users:x:100:
utmp:x:22:
utempter:x:35:
input:x:999:
systemd-journal:x:190:
systemd-network:x:192:
dbus:x:81:
polkitd:x:998:
libstoragemgmt:x:997:
ssh_keys:x:996:
rpc:x:32:
ntp:x:38:
abrt:x:173:
sshd:x:74:
slocate:x:21:
postdrop:x:90:
postfix:x:89:
chrony:x:995:
tcpdump:x:72:
stapusr:x:156:
stapsys:x:157:
stapdev:x:158:
syslog:x:994:
lighthouse:x:1000:lighthouse
weick:x:1001:
zhangsan:x:1002:
[root@VM-20-7-centos etc]# 

```



> 新增用户组

语法：groupadd 选项 用户组

可以使用的选项有：

- -g GID 指定新用户组的组标识号（GID）。
- -o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。

```bash
[root@VM-20-7-centos etc]# groupadd grouptest1
[root@VM-20-7-centos etc]# cat group //此命令向系统中增加了一个新组，新组的组标识号是在当前已有的最大组标识号的基础上加1。 
...
lighthouse:x:1000:lighthouse
weick:x:1001:
zhangsan:x:1002:
grouptest1:x:1003:
[root@VM-20-7-centos etc]#
```



> 删除用户组

语法：groupdel 用户组

```
[root@VM-20-7-centos etc]# groupdel zhangsan
[root@VM-20-7-centos etc]# cat group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
grouptest1:x:1003:
[root@VM-20-7-centos etc]# 

```



> 修改用户组

语法：groupmod 选项 用户组

常用的选项有： 

- -g GID 为用户组指定新的组标识号。
- -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
- -n新用户组 将用户组的名字改为新名字

```bash
[root@VM-20-7-centos etc]# groupmod -g 1002 grouptest1
[root@VM-20-7-centos etc]# cat group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
grouptest1:x:1002:
[root@VM-20-7-centos etc]# 
```

```bash
[root@VM-20-7-centos etc]# groupmod -g 1004 -n group1 grouptest1
[root@VM-20-7-centos etc]# cat group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
group1:x:1004:
[root@VM-20-7-centos etc]# 

```

>  将一个已有用户增加到一个已有用户组中

将一个已有用户 weick 增加到一个已有用户组 group1中，使此用户组成为该用户的附加用户组，可以使用带 -a 参数的 `usermod`  指令。-a 代表 append， 也就是将用户添加到新用户组中而不必离开原有的其他用户组。不过需要与 -G 选项配合使用：

```bash
# usermod -a -G group1 weick
```

```bash
[root@VM-20-7-centos home]# usermod -a -G group1 weick
[root@VM-20-7-centos home]# ls -l
total 16
drwx------ 5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x 2 root       root       4096 Feb 24 11:41 mytmpdoc
drwx------ 4 weick      weick      4096 Feb 23 23:13 weick
drwxr-xr-x 2 root       root       4096 Jan 14 11:27 www
[root@VM-20-7-centos home]# cat /etc/group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
group1:x:1004:weick
[root@VM-20-7-centos home]# 

```



> 如果要将一个用户从某个组中删除

```bash
gpasswd -d weick group1
```



```bash
[root@VM-20-7-centos home]# gpasswd -d weick group1
Removing user weick from group group1
[root@VM-20-7-centos home]# cat /etc/group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
group1:x:1004:
[root@VM-20-7-centos home]# 


```



> 如果要同时将 weick的主要用户组改为 group1，则直接使用 -g 选项：

```
# usermod -g group1 weick
```

```bash
[root@VM-20-7-centos home]# ls -l
total 16
drwx------ 5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x 2 root       root       4096 Feb 24 11:41 mytmpdoc
drwx------ 4 weick      weick      4096 Feb 23 23:13 weick
drwxr-xr-x 2 root       root       4096 Jan 14 11:27 www
[root@VM-20-7-centos home]# usermod -g group1 weick
[root@VM-20-7-centos home]# ls -l
total 16
drwx------ 5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x 2 root       root       4096 Feb 24 11:41 mytmpdoc
drwx------ 4 weick      group1     4096 Feb 23 23:13 weick
drwxr-xr-x 2 root       root       4096 Jan 14 11:27 www
[root@VM-20-7-centos home]# cat /etc/group
...
lighthouse:x:1000:lighthouse
weick:x:1001:
group1:x:1004:
[root@VM-20-7-centos home]# 
```

> 如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。

用户可以在登录后，使用命令newgrp切换到其他用户组，这个命令的参数就是目的用户组。

```bash
[root@VM-20-7-centos home]# su weick
[weick@VM-20-7-centos home]$ newgrp group1
[weick@VM-20-7-centos home]$ newgrp root
Password: 
newgrp: failed to crypt password with previous salt: Invalid argument
[weick@VM-20-7-centos home]$ newgrp group2
[weick@VM-20-7-centos home]$ 

```



> 管理用户组（group）的工具或命令

- groupadd    注：添加用户组；
- groupdel    注：删除用户组；
- groupmod    注：修改用户组信息
- groups      注：显示用户所属的用户组
- grpck
- grpconv     注：通过/etc/group和/etc/gshadow 的文件内容来同步或创建/etc/gshadow ，如果/etc/gshadow 不存在则创建；
- grpunconv   注：通过/etc/group 和/etc/gshadow 文件内容来同步或创建/etc/group ，然后删除gshadow文件；





用户组怎么做权限管理？后面再补充