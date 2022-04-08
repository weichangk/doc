##### 文件与目录

- ls（英文全拼：list files）: 列出目录及文件名

- cd（英文全拼：change directory）：切换目录

- pwd（英文全拼：print work directory）：显示目前的目录

- mkdir（英文全拼：make directory）：创建一个新的目录

- rmdir（英文全拼：remove directory）：删除一个空的目录

- cp（英文全拼：copy file）: 复制文件或目录

- rm（英文全拼：remove）: 删除文件或目录

- mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

>
>  ls
>

选项与参数

- -a  ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)
-  -d  ：仅列出目录本身，而不是列出目录内的文件数据(常用)
-  -l  ：长数据串列出，包含文件的属性与权限等等数据；(常用)

```shell
[root@VM-20-7-centos var]# ls
adm                cache  db     games   kerberos  local  log   nis  preserve  spool  yp
bt_setupPath.conf  crash  empty  gopher  lib       lock   mail  opt  run       tmp
[root@VM-20-7-centos var]# ls -a
.   adm                cache  db     games   kerberos  local  log   nis  preserve  spool  .updated
..  bt_setupPath.conf  crash  empty  gopher  lib       lock   mail  opt  run       tmp    yp
[root@VM-20-7-centos var]# ls -d
.
[root@VM-20-7-centos var]# ls -l
total 72
drwxr-xr-x.  2 root root 4096 Apr 11  2018 adm
-rw-r--r--   1 root root    4 Feb 19 18:59 bt_setupPath.conf
drwxr-xr-x.  6 root root 4096 Jan 14 11:23 cache
drwxr-xr-x.  2 root root 4096 Jun 10  2021 crash
drwxr-xr-x.  3 root root 4096 Nov 23 20:10 db
drwxr-xr-x.  3 root root 4096 Mar  7  2019 empty
drwxr-xr-x.  2 root root 4096 Apr 11  2018 games
drwxr-xr-x.  2 root root 4096 Apr 11  2018 gopher
drwxr-xr-x.  3 root root 4096 Dec  2 23:32 kerberos
drwxr-xr-x. 28 root root 4096 Jan 14 11:24 lib
drwxr-xr-x.  2 root root 4096 Apr 11  2018 local
lrwxrwxrwx.  1 root root   11 Mar  7  2019 lock -> ../run/lock
drwxr-xr-x. 10 root root 4096 Feb 20 03:15 log
lrwxrwxrwx.  1 root root   10 Mar  7  2019 mail -> spool/mail
drwxr-xr-x.  2 root root 4096 Apr 11  2018 nis
drwxr-xr-x.  2 root root 4096 Apr 11  2018 opt
drwxr-xr-x.  2 root root 4096 Apr 11  2018 preserve
lrwxrwxrwx.  1 root root    6 Mar  7  2019 run -> ../run
drwxr-xr-x. 11 root root 4096 Mar  7  2019 spool
drwxrwxrwt.  5 root root 4096 Feb 19 18:59 tmp
drwxr-xr-x.  2 root root 4096 Apr 11  2018 yp
[root@VM-20-7-centos var]# ls -al
total 84
drwxr-xr-x. 19 root root 4096 Jan 14 11:27 .
dr-xr-xr-x. 20 root root 4096 Feb 22 21:52 ..
drwxr-xr-x.  2 root root 4096 Apr 11  2018 adm
-rw-r--r--   1 root root    4 Feb 19 18:59 bt_setupPath.conf
drwxr-xr-x.  6 root root 4096 Jan 14 11:23 cache
drwxr-xr-x.  2 root root 4096 Jun 10  2021 crash
drwxr-xr-x.  3 root root 4096 Nov 23 20:10 db
drwxr-xr-x.  3 root root 4096 Mar  7  2019 empty
drwxr-xr-x.  2 root root 4096 Apr 11  2018 games
drwxr-xr-x.  2 root root 4096 Apr 11  2018 gopher
drwxr-xr-x.  3 root root 4096 Dec  2 23:32 kerberos
drwxr-xr-x. 28 root root 4096 Jan 14 11:24 lib
drwxr-xr-x.  2 root root 4096 Apr 11  2018 local
lrwxrwxrwx.  1 root root   11 Mar  7  2019 lock -> ../run/lock
drwxr-xr-x. 10 root root 4096 Feb 20 03:15 log
lrwxrwxrwx.  1 root root   10 Mar  7  2019 mail -> spool/mail
drwxr-xr-x.  2 root root 4096 Apr 11  2018 nis
drwxr-xr-x.  2 root root 4096 Apr 11  2018 opt
drwxr-xr-x.  2 root root 4096 Apr 11  2018 preserve
lrwxrwxrwx.  1 root root    6 Mar  7  2019 run -> ../run
drwxr-xr-x. 11 root root 4096 Mar  7  2019 spool
drwxrwxrwt.  5 root root 4096 Feb 19 18:59 tmp
-rw-r--r--   1 root root  163 Mar  7  2019 .updated
drwxr-xr-x.  2 root root 4096 Apr 11  2018 yp
```

 

> cd [相对路径或绝对路径]
>

```shell
[root@VM-20-7-centos var]# ls
adm                cache  db     games   kerberos  local  log   nis  preserve  spool  yp
bt_setupPath.conf  crash  empty  gopher  lib       lock   mail  opt  run       tmp
[root@VM-20-7-centos var]# cd log
[root@VM-20-7-centos log]# cd ./var/lib
-bash: cd: ./var/lib: No such file or directory
[root@VM-20-7-centos log]# cd /var/lib
[root@VM-20-7-centos lib]# 
```



> pwd 
>

选项与参数：

-  -P ：显示出确实的路径，而非使用连结 (link) 路径。 

```shell
[root@VM-20-7-centos lib]# pwd
/var/lib
```



> mkdir
>

选项与参数

-  -m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～
-  -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！

```shell
[root@VM-20-7-centos home]# mkdir foo
[root@VM-20-7-centos home]# ls
foo  lighthouse  www
[root@VM-20-7-centos home]# mkdir foo
mkdir: cannot create directory ‘foo’: File exists
[root@VM-20-7-centos home]# mkdir foo/fooo
[root@VM-20-7-centos home]# mkdir foo/fooo/foooo/fooooo
mkdir: cannot create directory ‘foo/fooo/foooo/fooooo’: No such file or directory
[root@VM-20-7-centos home]# mkdir -p foo/fooo/foooo/fooooo
[root@VM-20-7-centos home]# mkdir defaultAuthFolder
[root@VM-20-7-centos home]# mkdir -m 711  CustomAuthFolder
[root@VM-20-7-centos home]# ls -l
total 20
drwx--x--x 2 root       root       4096 Feb 22 22:12 CustomAuthFolder
drwxr-xr-x 2 root       root       4096 Feb 22 22:11 defaultAuthFolder
drwxr-xr-x 3 root       root       4096 Feb 22 22:06 foo
drwx------ 5 lighthouse lighthouse 4096 Feb 15 19:40 lighthouse
drwxr-xr-x 2 root       root       4096 Jan 14 11:27 www


```



>   rmdir
>

选项与参数：

-  **-p ：**从该目录起，一次删除多级空目录 

```shell
[root@VM-20-7-centos home]# rmdir foo
rmdir: failed to remove ‘foo’: Directory not empty
[root@VM-20-7-centos home]# rmdir -p foo/fooo
rmdir: failed to remove ‘foo/fooo’: Directory not empty
[root@VM-20-7-centos home]# rmdir -p foo/fooo/foooo/fooooo
[root@VM-20-7-centos home]# ls
CustomAuthFolder  defaultAuthFolder  lighthouse  www
[root@VM-20-7-centos home]# mkdir CustomAuthFolder/test
[root@VM-20-7-centos home]# cd CustomAuthFolder/test ls
[root@VM-20-7-centos test]# mkdir helloworld
[root@VM-20-7-centos test]# cd /home/ CustomAuthFolder/test ls
[root@VM-20-7-centos home]# cd /home/CustomAuthFolder/test ls
[root@VM-20-7-centos test]# ls
helloworld
[root@VM-20-7-centos test]# cd ..
[root@VM-20-7-centos CustomAuthFolder]# ls
test
[root@VM-20-7-centos CustomAuthFolder]# rmdir test/
rmdir: failed to remove ‘test/’: Directory not empty
[root@VM-20-7-centos CustomAuthFolder]# rmdir -p test/
rmdir: failed to remove ‘test/’: Directory not empty
[root@VM-20-7-centos CustomAuthFolder]# rmdir -f test/
rmdir: invalid option -- 'f'
Try 'rmdir --help' for more information.
[root@VM-20-7-centos CustomAuthFolder]# rmdir test/helloworld
[root@VM-20-7-centos CustomAuthFolder]# ls
test


```



