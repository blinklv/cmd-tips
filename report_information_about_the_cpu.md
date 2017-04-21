# Report information about the cpu

**lscpu** is used to display information about the CPU architecture, there are  
few options for this command. I only just introduce the default mode, but the  
main problem is understanding the meaning of the output information.

# Default

```bash
$lscpu

Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
CPU socket(s):         1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 30
Stepping:              5
CPU MHz:               2534.000
BogoMIPS:              5054.19
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              8192K
NUMA node0 CPU(s):     0-7
```

- **Architecture**: The current kernel architecture (64bit).
- **CPU op-mode**: CPU supports both 32 bit and 64 bit instructions. 
- **Byte Order**: [Endianness]
- **CPU op-mode**: my CPU supports both 32 bit and 64 bit instructions.
- **CPU(s)**: The logical CPU (not physical CPU) number of a CPU as used by the Linux kernel.   
  It equals to Core(s) times Thread(s) in my machine.
- **On-line CPU(s) list**: The CPU list of which the Linux instance makes use.
- **Thread(s) per core**: The number thread in each CPU core. You can know about [Hyper-threading] technology, which will help you understand this item.
- **Core(s) per socket**: The number of core in each [CPU socket].
- **CPU socket(s)**: The number of logical [CPU socket].
- **NUMA node(s)**: The number of NUMA node. [The more detail about **NUMA**](https://en.wikipedia.org/wiki/Non-uniform_memory_access)

[Endianness]: https://en.wikipedia.org/wiki/Endianness
[Hyper-threading]: https://en.wikipedia.org/wiki/Hyper-threading 
[CPU socket]: https://en.wikipedia.org/wiki/CPU_socket
