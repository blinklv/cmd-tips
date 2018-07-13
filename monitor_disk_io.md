# Monitor Disk I/O Activity and Usage

### iotop

[iotop][] iotop displays columns for the I/O bandwidth read and written by each process/thread during the sampling period. It's similar to **top** command, but it's simpler than that. This tip only introduces some options and ways I usually use.

```bash
$ iotop -Poa 

Total DISK READ: 0.00 B/s | Total DISK WRITE: 133.83 K/s
  PID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND
 2022 be/4 root          0.00 B     76.00 K  0.00 %  0.02 % [kjournald]
 1329 be/4 root          0.00 B      4.00 K  0.00 %  0.01 % [kjournald]
 2024 be/4 root          0.00 B      0.00 B  0.00 %  0.01 % [kjournald]
23458 be/4 root          0.00 B      0.00 B  0.00 %  0.00 % ./phone2tv_bind_svr
23452 be/4 root          0.00 B      4.00 K  0.00 %  0.00 % ./video_projection_svr
 4704 ?dif root          0.00 B      8.00 K  0.00 %  0.00 % ./l5_agent ../conf/config.xml -d
11453 be/4 root          0.00 B     12.00 K  0.00 %  0.00 % ons_agent
 4795 be/4 root          0.00 B      4.00 K  0.00 %  0.00 % ./l5_config
11511 be/4 root          0.00 B      4.00 K  0.00 %  0.00 % ops_14 14 /usr/local/ons_agent/plugins/ops_14.pid /usr/loc~_14.conf /usr/local/ons_agent/bin/ops_monitor_socket ops_14
 3115 be/4 root          0.00 B      4.00 K  0.00 %  0.00 % rsyslogd -i /var/run/syslogd.pid -c 4
13445 be/4 root          0.00 B      4.00 K  0.00 %  0.00 % ./dcagent ../etc/config.xml dcagent
25504 be/4 root          0.00 B     72.00 K  0.00 %  0.00 % sap1002
```

The `-P` option and the `-o` option make **iotop** only show processes actually doing I/O. The `-a` option, show accumulated I/O instead of bandwidth. In this mode, **iotop** shows the amount of I/O processes have done since iotop started. Use the `left` and `right` arrows to change the sorting; the following results are sorted by using **DISK WRITE** as the primary key.

```bash
Total DISK READ: 0.00 B/s | Total DISK WRITE: 0.00 B/s
  PID  PRIO  USER     DISK READ DISK WRITE>  SWAPIN      IO    COMMAND
 2022 be/4 root          0.00 B    744.00 K  0.00 %  0.05 % [kjournald]
10768 be/4 root          0.00 B    260.00 K  0.00 %  0.00 % ./logpool
 1329 be/4 root          4.00 K    188.00 K  0.00 %  0.02 % [kjournald]
 2024 be/4 root          0.00 B    140.00 K  0.00 %  0.01 % [kjournald]
14354 be/4 root          0.00 B     84.00 K  0.00 %  0.00 % TsysProxy
```


### iostat

[iostat][] can report CPU statistics and input/output statistics for devices, partitions and NFS, but I only introduce the following way to report I/O statistics periodically (every 1 second).

```bash
$ iostat -xdm 1

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.12   108.21    0.45   17.39     0.00     0.49    56.83     0.00    2.52   1.16   2.06

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               3.00    61.00   45.00    4.00     0.19     0.25    18.45     0.96   19.59   1.31   6.40

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00    34.00    0.00    3.00     0.00     0.14    98.67     0.00    1.33   1.33   0.40

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00    35.00    0.00   12.00     0.00     0.18    31.33     0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00     7.00    0.00   11.00     0.00     0.07    13.09     0.02    1.82   0.73   0.80

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00    75.00    0.00    2.00     0.00     0.30   308.00     0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00    45.00    0.00    4.00     0.00     0.19    98.00     0.00    1.00   1.00   0.40

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00   0.00   0.00

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00  2067.00    0.00  143.00     0.00     8.53   122.18     7.26   43.69   1.01  14.40

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00     0.00    0.00    8.00     0.00     0.13    34.00     0.06  133.00   2.00   1.60

Device:         rrqm/s   wrqm/s     r/s     w/s    rMB/s    wMB/s avgrq-sz avgqu-sz   await  svctm  %util
sda               0.00    77.00    0.00    4.00     0.00     0.32   162.00     0.00    1.00   1.00   0.40
```

[iotop]: https://linux.die.net/man/1/iotop
[iostat]: https://linux.die.net/man/1/iostat

