# Monitor network connection

## Summary Statistics

```bash
$ ss -s

Total: 380 (kernel 663)
TCP:   36339 (estab 192, closed 36129, orphaned 4, synrecv 0, timewait 36129/0), ports 768

Transport Total     IP        IPv6
*         663       -         -
RAW       0         0         0
UDP       27        27        0
TCP       210       190       20
INET      237       217       20
FRAG      0         0         0
```

## Show listening sockets

```bash
$ ss -ltn

Recv-Q Send-Q                                                              Local Address:Port                                                                Peer Address:Port
0      4096                                                                           :::8192                                                                          :::*
0      128                                                                10.134.144.153:56000                                                                          *:*
0      128                                                                10.134.144.153:36000                                                                          *:*
0      4096                                                               10.134.144.153:20005                                                                          *:*
0      4096                                                                    127.0.0.1:20005                                                                          *:*
0      100                                                                     127.0.0.1:42222                                                                          *:*
0      511                                                                             *:80                                                                             *:*
0      4096                                                               10.134.144.153:48369                                                                          *:*
0      1000                                                               10.134.144.153:8978                                                                           *:*
0      5                                                                  10.134.144.153:6868                                                                           *:*
0      1024                                                                    127.0.0.1:5210                                                                           *:*
0      511                                                                             *:443                                                                            *:*
```

Sometimes I like adding the **p** option, it will show which process binds this port.

## Filtering connections

### Basic Format

```bash
$ ss [ OPTIONS ] [ STATE-FILTER ] [ ADDRESS-FILTER ]
```

The state can be either of the following:

- established
- syn-sent
- syn-recv
- fin-wait-1
- fin-wait-2
- time-wait
- closed
- close-wait
- last-ack
- all - All of the above state.
- connected - All the state except for listen and closed.
- synchronized - All the connected states except syn-sent.
- bucket - Show states, which are maintained as minisockets, i.e. time-wait and syn-recv.
- big - Opposite to bucket state.

Maybe **established**, **time-wait** and **close-wait** are most common use.

### Get the established connections for the specific destination port

```bash
$ ss -tn4 state established dst :7002

Recv-Q Send-Q                      Local Address:Port                        Peer Address:Port
0      0                          10.134.144.153:50800                       10.240.85.99:7002
0      0                          10.134.144.153:38777                       10.49.128.35:7002
0      0                          10.134.144.153:38556                       10.49.128.35:7002
0      0                          10.134.144.153:51534                       10.240.85.99:7002
0      0                          10.134.144.153:38723                       10.49.128.35:7002
0      0                          10.134.144.153:38547                       10.49.128.35:7002
0      0                          10.134.144.153:46400                       10.240.85.99:7002
0      0                          10.134.144.153:54873                       10.240.85.99:7002
0      0                          10.134.144.153:65128                       10.49.128.35:7002
0      0                          10.134.144.153:40653                       10.49.128.35:7002
0      0                          10.134.144.153:34635                       10.49.128.35:7002
0      0                          10.134.144.153:53063                       10.240.85.99:7002
0      0                          10.134.144.153:34634                       10.49.128.35:7002
0      0                          10.134.144.153:38570                       10.49.128.35:7002
0      114                        10.134.144.153:58158                       10.240.85.99:7002
0      0                          10.134.144.153:48121                       10.240.85.99:7002
0      0                          10.134.144.153:38719                       10.49.128.35:7002
0      114                        10.134.144.153:38553                       10.49.128.35:7002
0      0                          10.134.144.153:50789                       10.240.85.99:7002
0      0                          10.134.144.153:50783                       10.240.85.99:7002
0      0                          10.134.144.153:34631                       10.49.128.35:7002
0      0                          10.134.144.153:56117                       10.49.128.35:7002
```

Above example only shows the TCP connections based on IPv4, if you want to include IPv6,    
remove the **4** option.

### Watch

I just want to get the informations of the connections in real time, repeating the command is boring.

```bash
$ watch -n 1 'ss -tn4 state time-wait dst :7002'
```

The screen will be refreshed in every second.
