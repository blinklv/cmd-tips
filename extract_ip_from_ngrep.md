# Extract IP from ngrep

Sometimes, A sudden increase in the amount of request to your HTTP server.  Maybe some clients have a loop bug, new business online or some people attack your server etc.
I think analyze request ip addresses is a good choice to solve this problem. But your HTTP server doesn't record access log, you don't have information about request.
OK, let's spot capture request packages and extract ip addresses to analyze.

## First Case

Yes, there're not access proxies server in front of your HTTP server, the requests come from clients directly. The ip addresses you need to extract are remote addresses.
I just use the simplest regular expressiion (a single `HTTP` string) and filter (destination port 80) in **ngrep** command to demonstrate this process, you can modify 
them based on your needs.

```bash
sudo ngrep -d any -W byline -q 'HTTP' 'dst port 80' | sed  -rn 's/^T (([0-9]{1,3}\.){3}[0-9]{1,3}).*->.*\[AP\].*$/\1/p'
```

## Second Case

If some access proxies are downstream of your HTTP server, so the remote addresses are host of proxies. We can extract client ip addresses from HTTP header `X-Real-IP`.

```bash
sudo ngrep -d any -W byline -q 'HTTP' 'dst port 80' | sed  -rn 's/^X-Real-IP: (([0-9]{1,3}\.){3}[0-9]{1,3}).*$/\1/p'
```

## Third Case

Unfortunately, sometimes the contents of `X-Real-IP` header are the addresses of access proxies instead of real clients. But we also can get the addresses of real clients by 
using `X-Forwarded-For` header.

```bash
sudo ngrep -d any -W byline -q 'HTTP' 'dst port 80' | sed  -rn 's/^X-Forwarded-For: (([0-9]{1,3}\.){3}[0-9]{1,3}).*$/\1/p'
```

## Stats

We have already extracted the ip addresses of clients, we can do some simple stats on them as follows:

```bash
sudo ngrep -d any -W byline -q 'HTTP' 'dst port 80' | sed  -rn 's/^T (([0-9]{1,3}\.){3}[0-9]{1,3}).*\[AP\].*$/\1/p' > addresses.txt
sort addresses.txt | uniq -c | sort -r

   2849 10.60.23.195
   2443 10.61.216.151
   2093 10.61.212.219
   2050 10.61.212.220
   1266 10.60.146.25
   1254 10.60.22.143
   1186 10.61.21.152
   1178 10.60.82.194
   1130 10.233.143.176
   1070 10.60.76.210
    993 10.60.77.82
    959 10.238.1.77
    937 10.175.93.208
    935 10.175.109.141
    926 10.61.23.133
    919 10.49.96.214
    916 10.120.136.67
    865 10.49.128.75
    809 10.240.86.91
    789 10.238.1.76
    763 10.175.94.101
    745 10.238.2.207
    721 10.175.94.40
    719 10.229.9.82
    719 10.169.135.229
    685 10.175.109.143
    636 10.175.109.144
    602 10.235.19.41
    600 10.49.97.4
    597 10.49.116.88
    592 10.49.117.130
    562 10.49.99.232
    511 10.118.116.89
    499 10.198.132.154
    488 10.49.116.86
    483 10.49.116.87
    464 10.170.1.21
    455 10.49.116.89
    439 10.118.116.86
```
