# Check hard disk performance

## Read Speed

Using **hdparm** command, but you should have the *root* permission.

```bash
$ hdparm -tT /dev/sda

/dev/sda:
 Timing cached reads:   15596 MB in  2.00 seconds = 7805.35 MB/sec
 Timing buffered disk reads: 362 MB in  3.01 seconds = 120.33 MB/sec
```

- **Timing cached reads**: The speed of reading directly from the Linux buffer cache without disk access.
- **Timing buffered disk reads**: The speed of reading through the buffer cache to the disk without any prior caching of data.

You also can use **dd** command, and you don't need the **root** permission.

```bash
$dd if=./input of=/dev/null bs=64k iflag=direct

3200+0 records in
3200+0 records out
209715200 bytes (210 MB) copied, 1.6582 s, 126 MB/s
```
## Write Speed

**Throughput**

```bash
$dd if=/dev/zero of=./output bs=100M count=1 oflag=dsync

1+0 records in
1+0 records out
104857600 bytes (105 MB) copied, 0.999409 s, 105 MB/s
```

**Latency**

```bash
$dd if=/dev/zero of=./output bs=512 count=1000 oflag=dsync

1000+0 records in
1000+0 records out
512000 bytes (512 kB) copied, 0.54793 s, 934 kB/s
```

One thousand accesses required 0.55s, meaning one access took 0.55ms.
