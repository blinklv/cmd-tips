# nmap

**nmap** is short for **Network Mapper**. We can use it do something as follows:    


1. Find out computers runing on the local network
2. Find out IP on the local network
3. Find out OS on your target machine
4. Find out what ports are open on the machine you just scanned
5. Find out if the system is infected with malware or virus    
6. Search for unauthorized servers or network service on your network

## Scan IP   

**Scan a single host or an IP address**

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
$nmap 192.168.1.*
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

**Find out if a host/network is protected by a firewall**


```bash
$namp -sA 10.133.34.1

Starting Nmap 5.21 ( http://nmap.org ) at 2017-08-17 17:26 CST
Nmap scan report for 10.133.34.1
Host is up (0.00079s latency).
Not shown: 996 unfiltered ports
PORT      STATE    SERVICE
40911/tcp filtered unknown
41511/tcp filtered unknown
42510/tcp filtered unknown
44176/tcp filtered unknown
MAC Address: 00:0F:E2:FF:00:91 (Hangzhou H3C Technologies Co.)

Nmap done: 1 IP address (1 host up) scanned in 8.67 seconds
```

State **filtered** means it's protected by a firewall.

**Display the reason a port is in a particular state**


```bash
$nmap --reason 10.133.34.1

Starting Nmap 5.21 ( http://nmap.org ) at 2017-08-17 17:30 CST
Nmap scan report for 10.133.34.1
Host is up, received conn-refused (0.00087s latency).
Not shown: 994 closed ports
Reason: 994 conn-refused
PORT      STATE    SERVICE REASON
23/tcp    open     telnet  syn-ack
80/tcp    open     http    syn-ack
40911/tcp filtered unknown no-response
41511/tcp filtered unknown no-response
42510/tcp filtered unknown no-response
44176/tcp filtered unknown no-response

Nmap done: 1 IP address (1 host up) scanned in 6.76 seconds
```

**Scan specific ports**

```bash
$nmap -p 80 192.168.1.1
$nmap -p 10-20 192.168.1.1
```

## Show host interfaces and routes

```bash
$nmap --iflist

Starting Nmap 5.21 ( http://nmap.org ) at 2017-08-17 17:35 CST
************************INTERFACES************************
DEV     (SHORT)  IP/MASK          TYPE        UP MAC
lo      (lo)     127.0.0.1/8      loopback    up
eth1    (eth1)   10.133.34.24/26  ethernet    up 00:E0:81:D8:0B:59
tunl0:0 (tunl0)  59.37.116.33/32  other       up
tunl0:1 (tunl0)  163.177.73.30/32 other       up
tunnat  (tunnat) 127.0.0.53/32    point2point up
tunl1   (tunl1)  127.0.1.1/32     point2point up
tunl2   (tunl2)  127.0.1.2/32     point2point up

**************************ROUTES**************************
DST/MASK      DEV    GATEWAY
10.133.34.0/2 eth1
169.254.0.0/0 eth1
192.168.0.0/0 eth1   10.133.34.1
172.16.0.0/0  eth1   10.133.34.1
100.64.0.0/0  eth1   10.133.34.1
9.0.0.0/0     eth1   10.133.34.1
10.0.0.0/0    eth1   10.133.34.1
0.0.0.0/0     tunnat
```
