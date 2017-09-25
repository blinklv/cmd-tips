# Routing Loop

I want to access the host **211.138.236.210**, but it can't work. So I use **ping** command to    
test this ip address.        

```bash
$ ping 211.138.236.210

PING 211.138.236.210 (211.138.236.210) 56(84) bytes of data.
From 211.142.211.106 icmp_seq=1 Time to live exceeded
From 211.142.211.106 icmp_seq=2 Time to live exceeded
From 211.142.211.106 icmp_seq=3 Time to live exceeded
From 211.142.211.106 icmp_seq=4 Time to live exceeded
```

Wait! Time to live exceeded? What the fuck does this mean? And why the response packet comes      
from the ip **211.142.211.106**? Google it!

>The TTL time exceeded ICMP message is sent when the TTL value of an IP packet reaches zero.       
>In normal operation, a network should not have a diameter so great that the TTL gets reduced        
>to zero. The most common occurrence of this is when there is a routing loop. In this case,       
>as the packet is sent back and forth between the looping points, the TTL keeps getting        
>decremented until it reaches zero. 


Does this mean a [routing-loop problem](https://en.wikipedia.org/wiki/Routing_loop_problem) happen? OK, let me use **traceroute** command to verify it.     

```bash
$  traceroute 211.138.236.210
traceroute to 211.138.236.210 (211.138.236.210), 30 hops max, 60 byte packets
1  * * *
2  * * *
3  * * *
4  * * *
5  * * *
6  * * *
7  * * *
8  202.97.65.157 (202.97.65.157)  29.160 ms 202.97.64.1 (202.97.64.1)  25.233 ms 202.97.65.157 (202.97.65.157)  29.155 ms
9  * * *
10  221.183.30.89 (221.183.30.89)  27.084 ms 221.183.30.81 (221.183.30.81)  32.498 ms  32.084 ms
11  * 221.183.25.17 (221.183.25.17)  60.962 ms  52.653 ms
12  221.176.27.225 (221.176.27.225)  35.790 ms  33.079 ms 221.183.11.30 (221.183.11.30)  27.997 ms
13  221.183.26.214 (221.183.26.214)  34.712 ms 221.183.19.194 (221.183.19.194)  28.678 ms 221.176.20.234 (221.176.20.234)  34.674 ms
14  111.8.29.6 (111.8.29.6)  35.022 ms 211.142.209.85 (211.142.209.85)  37.163 ms *
15  111.8.18.210 (111.8.18.210)  36.374 ms 111.8.29.6 (111.8.29.6)  35.521 ms 111.8.18.213 (111.8.18.213)  36.991 ms
16  * 211.142.211.106 (211.142.211.106)  29.374 ms 111.8.18.213 (111.8.18.213)  34.630 ms
17  211.142.211.106 (211.142.211.106)  31.115 ms * *
18  * * *
19  211.142.211.106 (211.142.211.106)  44.331 ms  48.353 ms *
20  * 211.142.211.106 (211.142.211.106)  44.806 ms *
21  211.142.211.106 (211.142.211.106)  47.858 ms *  45.235 ms
22  * 211.142.211.106 (211.142.211.106)  46.669 ms  46.166 ms
23  * * 211.142.211.106 (211.142.211.106)  45.655 ms
24  211.142.211.106 (211.142.211.106)  47.035 ms * *
25  211.142.211.106 (211.142.211.106)  34.796 ms *  39.478 ms
26  * 211.142.211.106 (211.142.211.106)  34.292 ms *
27  211.142.211.106 (211.142.211.106)  40.005 ms  32.905 ms  36.385 ms
28  * * *
29  * * 211.142.211.106 (211.142.211.106)  32.544 ms
30  * * *
```

It really exists a routing-loop problem at ip **211.142.211.106**.
