# nmap

**nmap** is short for **Network Mapper**. We can use it do something as follows:    


1. Find out computers runing on the local network
2. Find out IP on the local network
3. Find out OS on your target machine
4. Find out what ports are open on the machine you just scanned
5. Find out if the system is infected with malware or virus    
6. Search for unauthorized servers or network service on your network

## Scan IP   

**Scan a single host or an IP address

```bash
$nmap 10.133.34.1

Starting Nmap 5.21 ( http://nmap.org ) at 2017-08-17 11:27 CST
Nmap scan report for 10.133.34.1
Host is up (0.0024s latency).
Not shown: 994 closed ports
PORT      STATE    SERVICE
23/tcp    open     telnet
80/tcp    open     http
40911/tcp filtered unknown
41511/tcp filtered unknown
42510/tcp filtered unknown
44176/tcp filtered unknown

Nmap done: 1 IP address (1 host up) scanned in 8.87 seconds

$nmap www.baidu.com

Starting Nmap 5.21 ( http://nmap.org ) at 2017-08-17 11:28 CST
Nmap scan report for www.baidu.com (14.215.177.37)
Host is up (0.0084s latency).
Hostname www.baidu.com resolves to 2 IPs. Only scanned 14.215.177.37
Not shown: 998 filtered ports
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 4.58 seconds
```

**Scan multiple IP address or subnet**

```bash
$nmap 192.168.1.1 192.168.1.2 192.168.1.3
$nmap 192.168.1.1,2,3    
$nmap 192.168.1.1-20   
$nmap 192.168.1.\*
$nmap 192.168.1.0/24     
```

**Read list of hosts/networks froma a file**    

This is useful to scan a large number of hosts/networks.

```bash
$nmap -iL /tmp/ip.txt
```

**Excluding hosts/networks**

```bash
$nmap 192.168.1.0/24 --exclude 192.168.1.5,192.168.1.254
$nmap -iL /tmp/ip.txt --excludefile /tmp/exclude.txt
```
