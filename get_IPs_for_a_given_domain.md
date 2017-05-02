# Get IPs for a given domain

You can use the **dig** command to achieve it, but the answer   
is based on domain server you access. If you want to gel all   
IPs for a domain, sadly, there is no easy way to do this.

## The Basic Format 

```bash
dig @dns-server domain type
```
- **dns-server**: The name or IP address of the name server, if this arugment is not   
provided, **dig** consults */etc/resolv.conf* and quires the name servers listed there.
- **domain**: The domain name.
- **type**: Indicates what type of query is required - ANY, A, MX, SIG, etc.

## Example

```bash
$dig +short @8.8.8.8 tencent.com A

;; Truncated, retrying in TCP mode.
113.105.73.141
183.56.150.142
113.107.238.27
113.107.238.12
113.107.238.15
119.147.253.25
113.107.238.11
113.105.73.148
119.147.33.36
183.56.150.144
113.105.73.142
183.56.150.146
119.147.253.23
183.56.150.155
113.107.238.14
119.147.33.33
```

The **A** type means **Address**, and the **+short** option will make the answer in   
terse format. Now if I don't provide the **dns-name** (@8.8.8.8) argument, as below.

```bash
$dig +short tencent.com A

;; Truncated, retrying in TCP mode.
10.14.0.135
10.14.0.136
10.6.18.41
10.14.0.161
10.20.204.27
10.14.198.19
10.14.0.130
10.14.5.66
10.1.156.78
10.14.0.134
10.14.198.101
10.6.18.42
10.14.198.30
10.14.198.31
10.14.6.37
10.14.5.11
10.14.0.133
10.3.82.98
10.33.25.62
10.14.198.17
10.14.198.12
10.20.204.56
10.11.56.22
10.14.198.15
192.168.1.26
10.24.32.29
10.1.156.79
10.14.198.11
192.168.1.86
10.14.198.29
10.11.56.23
10.33.25.63
10.3.82.99
10.14.0.160
10.14.0.131
10.24.32.30
10.6.18.199
10.14.198.20
10.14.198.24
```

The answer is completely different from the first example, because **dig** will    
use the name servers listed in */etc/resolv.conf* file by default.
