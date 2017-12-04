# Too many open files

Recently, I published a server based on TCP custom protocol. It will hold some persistent connections     
with downstream servers, downstream servers will close a connection actively when some exceptions    
happen. At first, the server was running well, but a new connection can't be accepted after about a    
month. My log records a error **accept4: too many open files**. We know every process has a limit of     
open files, maybe this thresold is too low. **ulimit** command can answer this question.

```bash
$ ulimit -a

core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 385060
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 100001
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 385060
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

I only focus on **open files** term, 100001 is not a little number, there must be some file resource leaks     
in my server, but how do I find them. **lsof** is a good choice. 

```bash
$ lsof -p $(pidof process_name) 

view_hist 4448 user_00 *775u  IPv6 1677869851      0t0        TCP 10.223.134.43:9194->10.223.134.43:34370 (CLOSE_WAIT)
view_hist 4448 user_00 *776u  IPv6 1677909520      0t0        TCP 10.223.134.43:9194->10.224.139.136:59664 (ESTABLISHED)
view_hist 4448 user_00 *777u  IPv6 1677919476      0t0        TCP 10.223.134.43:9194->10.224.139.136:59747 (ESTABLISHED)
view_hist 4448 user_00 *778u  IPv6 1677920590      0t0        TCP 10.223.134.43:9194->10.223.134.43:34431 (CLOSE_WAIT)
view_hist 4448 user_00 *779u  IPv6 1677929750      0t0        TCP 10.223.134.43:9194->10.224.139.136:59823 (ESTABLISHED)
view_hist 4448 user_00 *780u  IPv6 1677953884      0t0        TCP 10.223.134.43:9194->10.223.134.43:34471 (CLOSE_WAIT)
view_hist 4448 user_00 *781u  IPv6 1678011652      0t0        TCP 10.223.134.43:9194->10.223.134.43:34541 (CLOSE_WAIT)
view_hist 4448 user_00 *782u  IPv6 1678046159      0t0        TCP 10.223.134.43:9194->10.223.134.43:34573 (CLOSE_WAIT)
view_hist 4448 user_00 *783u  IPv6 1678108941      0t0        TCP 10.223.134.43:9194->10.223.134.43:51006 (CLOSE_WAIT)
view_hist 4448 user_00 *784u  IPv6 1678112198      0t0        TCP 10.223.134.43:9194->10.49.95.67:27316 (ESTABLISHED)
view_hist 4448 user_00 *785u  IPv6 1678125742      0t0        TCP 10.223.134.43:9194->10.223.134.43:51018 (CLOSE_WAIT)
view_hist 4448 user_00 *786u  IPv6 1678133123      0t0        TCP 10.223.134.43:9194->10.224.133.153:53743 (ESTABLISHED)
view_hist 4448 user_00 *787u  IPv6 1678133145      0t0        TCP 10.223.134.43:9194->10.224.133.153:53744 (ESTABLISHED)
view_hist 4448 user_00 *788u  IPv6 1678136844      0t0        TCP 10.223.134.43:9194->10.223.134.43:51035 (CLOSE_WAIT)
view_hist 4448 user_00 *789u  IPv6 1678142847      0t0        TCP 10.223.134.43:9194->10.235.23.18:29189 (ESTABLISHED)
```

Wait! Why there're so many **CLOSE_WAIT** sockets ([Everything is a file](https://en.wikipedia.org/wiki/Everything_is_a_file))? How many **CLOSE_WAIT** sockets are there?       

```bash
$ lsof -p ($pidof process_name) | grep 'CLOSE_WAIT' -c

8789
```

I spot checked my codes, as I think, I forget closing connections.
