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
```

## List Specific Devices

If you want to get information about a specific device only, add the `-b` option   
and the device name.

```bash
$lsblk -b /dev/sda
```
