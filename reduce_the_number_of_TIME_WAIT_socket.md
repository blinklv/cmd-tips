# Reduce the number of TIME-WAIT socket

## The function of TIME-WAIT

There are two purposes for the **TIME-WAIT** state:

1. The most known one is to prevent delayed segments from one connection being accepted   
by a later connection relying on the same quadruplet (*src addr*, *src port*, *dst addr*, *dst port*).   

2. The other purpose is to ensure the remote end has closed the connection. When the last   
*ACK* is lost, the remote end stays in the **LAST-ACK** state, opening a new connection  
with some quadruplet will not work.

## The problems of TIME-WAIT

Yes, everything has two sides, include **TIME-WAIT** state.

1. The slot taken in the connection table preventing new connections of the same kind.
2. The memory occupied by the socket structure in the kernel (but it's really small).
3. The additional cpu usage.

## How to solve these problems?

In fact, the good way to solve it is increasing the one of quadruplet:

1. Use **more client ports** by setting `net.ipv4.ip_local_port_range` to a wider range.

2. Use **more server ports** by asking the server to listen to several additional ports.   
3. Use **more client IP** by configuring additional IP on the load balancer and use them in a round-robin fashion.
4. Use **more server IP** by configuring additional IP on the server.

Ok, It's not the theme of this note because it can't reduce the number of TIME-WAIT state.   
I will tweak `net.ipv4.tcp_tw_reuse` and `net.ipv4.tcp_tw_recycle` to achieve this goal.

## tcp_tw_reuse and tcp_tw_recycle 

Before I introduce these two system flags, you should know about [TCP Timestamps Option].   
It can help TCP determine in which order packets are sent.

### tcp_tw_reuse

> Allow to reuse TIME-WAIT sockets for new connection when it is safe from protocol viewpoint.    
Default value is 0. It should not be changed without advice/request of technical experts.

This description is from [Linux Documentation][tcp_tw_reuse]. What's the advice/request of  
technical experts? We need to dig up more valuable information.


By enabling this flag, Linux will reuse an existing connection in the **TIME-WAIT** state   
for a new *outgoing connection* if the new timestamp is strictly bigger than the most   
recent timestamp recorded for the previous connection. Thanks to the use of timestamps,  
a duplicate segments will come with an outdated timestamp and therefore be discarded,  
we can don't depend on the protection of **TIME-WAIT** state.

### tcp_tw_recycle

The official description of this flag as below (from [Linux Documention][tcp_tw_recycle]).

> Enable fast recycling TIME-WAIT sockets. Default value is 0. It should not be changed  
without advice/request of technical experts.

The detail about this flag is less than last one :joy:. In fact, this mechanism also relies on    
timestamp option but affects *both incoming and outgoing connections*. It means not only affect   
client side, but also server-side. The principle is similar to **tcp_tw_reuse**. There is a       
common problem about this flag when you run a server, and server-side closes connection actively,    
but the client side is a [**NAT**][NAT] device. some real client (in front of NAT) can't connect to    
server, because many client use common outbound ip and port through NAT, but they don't share the    
same timestamp clock, the same ip:port pair doesn't have increasing timestamp from viewport of server,    
the *SYN* packet will be silently dropped, and the connection won't establish (you will see an    
error similar to **connect timeout**).

## Enable these two flags

```bash
$sudo echo 1 > /proc/sys/net/ipv4/tcp_tw_reuse
$sudo echo 1 > /proc/sys/net/ipv4/tcp_tw_recycle
```

You should ensure that the **tcp_timestamps** flag has been set. You only need to set either **tcp_tw_reuse**   
or **tcp_tw_recycle**, not both.


[TCP Timestamps Option]: https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_timestamps
[tcp_tw_reuse]: http://lxr.linux.no/linux+v3.2.8/Documentation/networking/ip-sysctl.txt#L464
[tcp_tw_recycle]: http://lxr.linux.no/linux+v3.2.8/Documentation/networking/ip-sysctl.txt#L459
[NAT]: https://en.wikipedia.org/wiki/Network_address_translation
