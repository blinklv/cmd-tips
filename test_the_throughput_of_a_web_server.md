# Test the through of a web server.

I use **http_load** command, maybe you need to install [it](https://acme.com/software/http_load/) manually.

## Install

```bash
tar -xvzf http_load-09Mar2016.tar.gz
cd http_load-09Mar2016/
make
```

By default, HTTPS support is disabled. We can enable the HTTPS by   
uncomment the lines below in the Makefile.

```makefile
#SSL_TREE =     /usr/local/ssl
#SSL_DEFS =     -DUSE_SSL
#SSL_INC =      -I$(SSL_TREE)/include
#SSL_LIBS =     -L$(SSL_TREE)/lib -lssl –lcrypto
```

**NOTE**: If you want to enable the HTTPS mode, you should have already install the openssl.


## Test

```bash
$ ./http_load -parallel 10 -seconds 10 url.txt

74786 fetches, 10 max parallel, 2.76708e+06 bytes, in 10 seconds
37 mean bytes/connection
7478.57 fetches/sec, 276707 bytes/sec
msecs/connect: 0.077154 mean, 1.755 max, 0.013 min
msecs/first-response: 1.16992 mean, 689.982 max, 0.133 min
HTTP response codes:
  code 200 -- 74786
```

The **fetches/sec** and **bytes/sec** are metrics I most interest. The number after the **code 200** at the bottom represents there are 74786 http responses is ok, but it' not always like this, such as below:

```bash
$ ./http_load -parallel 10 -fetches 1000 url.txt  2>/dev/null

1000 fetches, 10 max parallel, 4.04215e+07 bytes, in 94.7155 seconds
40421.5 mean bytes/connection
10.5579 fetches/sec, 426767 bytes/sec
msecs/connect: 137.709 mean, 910.291 max, 35.067 min
msecs/first-response: 487.805 mean, 1595.15 max, 117.102 min
955 bad byte counts
HTTP response codes:
  code 200 -- 508
  code 403 -- 492
```

There are almost half of http response code is [403](https://en.wikipedia.org/wiki/HTTP_403), maybe the server has some protections.
