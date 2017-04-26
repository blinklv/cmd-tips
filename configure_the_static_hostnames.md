# Configure the static hostnames

Open the `/etc/hosts` file, you should have permission to edit it.

```bash
$sudo vi /etc/hosts
```

This file is a simple text file that associates IP addresses with    
hostnames, one line per IP address. For each host a single line    
should be present with the following information:

> IP_address canonical_hostname [aliases...]

**Example:**

```
# The following lines are desirable for IPv4 capable hosts
127.0.0.1       localhost

# 127.0.1.1 is often used for the FQDN of the machine
127.0.1.1       thishost.mydomain.org  thishost
192.168.1.10    foo.mydomain.org       foo
192.168.1.13    bar.mydomain.org       bar
146.82.138.7    master.debian.org      master
209.237.226.90  www.opensource.org

# The following lines are desirable for IPv6 capable hosts
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
```

You can change/add/delete a entry in this file to configure the static hostnames.

**NOTE**: Text from a `#` character until the end of the line is a comment.
