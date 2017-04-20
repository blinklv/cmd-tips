# List the block device information

The command **lsblk** lists information about all available or the specified block  
devices (except RAM disks).


## Default

```bash
$lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 465.8G  0 disk
|-sda1   8:1    0    10G  0 part /
|-sda2   8:2    0     2G  0 part [SWAP]
|-sda3   8:3    0    20G  0 part /usr/local
`-sda4   8:4    0 433.8G  0 part /data
```

- **NAME**: device name.
- **MAJ:MIN**: major and minor device number.
- **RM**: whether the device is removeable or not.
- **SIZE**: the size of the device.
- **RO**: whether the device is read-only.
- **TYPE**: device type.
- **MOUNTPOINT**: mount point on wich the device is mounted.


## List All Devices

The default option doesn't list all empty devices, to view these you only just add  
the `-a` option, it's easy, right? :wink:

```bash
$lsblk -a

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
ram0     1:0    0   125M  0 disk
ram1     1:1    0   125M  0 disk
ram2     1:2    0   125M  0 disk
ram3     1:3    0   125M  0 disk
ram4     1:4    0   125M  0 disk
ram5     1:5    0   125M  0 disk
ram6     1:6    0   125M  0 disk
ram7     1:7    0   125M  0 disk
ram8     1:8    0   125M  0 disk
ram9     1:9    0   125M  0 disk
ram10    1:10   0   125M  0 disk
ram11    1:11   0   125M  0 disk
ram12    1:12   0   125M  0 disk
ram13    1:13   0   125M  0 disk
ram14    1:14   0   125M  0 disk
ram15    1:15   0   125M  0 disk
loop0    7:0    0         0 loop
loop1    7:1    0         0 loop
loop2    7:2    0         0 loop
loop3    7:3    0         0 loop
loop4    7:4    0         0 loop
loop5    7:5    0         0 loop
loop6    7:6    0         0 loop
loop7    7:7    0         0 loop
sda      8:0    0 465.8G  0 disk
|-sda1   8:1    0    10G  0 part /
|-sda2   8:2    0     2G  0 part [SWAP]
|-sda3   8:3    0    20G  0 part /usr/local
`-sda4   8:4    0 433.8G  0 part /data
md0      9:0    0         0 md
```

