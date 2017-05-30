# Reduce the number of TIME-WAIT socket

## The function of TIME-WAIT

There are tow purposes for the **TIME-WAIT** state:

1. The most known one is to prevent delayed segments from one connection being accepted   
by a later connection relying on the same quadruplet (*src addr*, *src port*, *dst addr*, *dst port*).   

2. The other purpose is to ensure the remote end has closed the connection. When the last   
*ACK* is lost, the remote end stays in the **LAST-ACK** state, opening a new connection  
with the some quadruplet will not work.

## The problems of TIME-WAIT

Yes, everything has two sides, include **TIME-WAIT** state.

1. The slot taken in the connection table preventing new connections of the same kind.
2. The memory occupied by the socket structure in the kernel.
3. The additional cpu usage.

## How to solve these problems?

In fact, the good way to solve it is increasing the one of quadruplet:

1. Use **more client ports** by setting `net.ipv4.ip_local_port_range` to a wider range.

2. Use **more server ports** by asking the server to listen to serveral additional ports.   
3. Use **more client IP** by configuring additional IP on the load balancer and use them in a round-robin fashion.
4. Use **more server IP** by configuring additional IP on the server.

Ok, It's not the theme of this note, because it can't reduce the number of TIME-WAIT state. I will tweak `net.ipv4.tcp_tw_reuse` and `net.ipv4.tcp_tw_recycle` to achieve this goal.

