# Disk usage

Introduce **df (disk filesystem)** command and **du (disk usage)** command.

## Check file system disk space usage

The **df** command displays the information of device name, total blocks, total   
disk space, used disk space, available disk space and mount points on a file system.

```bash
$ df -h

Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              20G  2.3G   17G  13% /
/dev/sda3              20G  1.8G   17G  10% /usr/local
/dev/sda4             418G   15G  382G   4% /data
tmpfs                 3.9G     0  3.9G   0% /dev/shm
```

I like add `-h` option, it makes the result more readable.

## Display information of all file system disk space usage

The same as above, but it also displays information of dummy file system along with   
all the file system disk usage and their memory utilization.

```bash
$ df -ah

Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              20G  2.3G   17G  13% /
proc                     0     0     0   -  /proc
sysfs                    0     0     0   -  /sys
devpts                   0     0     0   -  /dev/pts
/dev/sda3              20G  1.8G   17G  10% /usr/local
/dev/sda4             418G   15G  382G   4% /data
tmpfs                 3.9G     0  3.9G   0% /dev/shm
none                     0     0     0   -  /proc/sys/fs/binfmt_misc
```

## Display the usage summary of a directory and each of its sub directories

```bash
$ du -h .

4.0K  ./admin/data/backup
4.0K  ./admin/data/tmp
24K ./admin/data
92K ./admin
4.0K  ./lib
52K ./conf
13M ./bin
14M .
```

The output of the above command displays the usage summary of the current directory    
and each of its sub directories. I like add `-h` option, it makes result more readable.


## Get the summary of a grand total disk usage of an directory

```bash
$ du -sh /lib

41M /lib
```

## Display the disk usage of all the files and directories

```bash
$ du -ah .

4.0K  ./admin/crontab.sh
12K ./admin/md5sum.sh
8.0K  ./admin/monitor.sh
4.0K  ./admin/start.sh
4.0K  ./admin/addcrontab.sh
4.0K  ./admin/stop.sh
4.0K  ./admin/data/backup
4.0K  ./admin/data/clear.conf
4.0K  ./admin/data/tmp
4.0K  ./admin/data/version.ini
4.0K  ./admin/data/source.ini
24K ./admin/data
4.0K  ./admin/restart.sh
8.0K  ./admin/clear.sh
8.0K  ./admin/uninstall.sh
8.0K  ./admin/common.sh
92K ./admin
28K ./update.sh
4.0K  ./lib
0 ./log
4.0K  ./ugrade_before.sh
48K ./conf/redis.conf
52K ./conf
5.5M  ./bin/redis_cli
7.5M  ./bin/redis_server
13M ./bin
8.0K  ./init.xml
4.0K  ./ugrade_after.sh
14M .
```

Maybe you don't want to display the disk usage recursively, you can use `--max-depth` option. 

```bash
$ du -h --max-depth=1 .

 68K./vim
2.4M./note
5.8M./py
1.2G./go
 11M./css
411M./js
172M./web
 24K./script
112M./c
1.9G.
```

## Find the biggest N files and directories of a directory in Linux.

```bash
$ du --max-depth=1 /Devel | sort -nr | head -n 5

4010632.
2547592./go
841056./js
353080./web
229080./c
``` 
