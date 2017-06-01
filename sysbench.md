# sysbench

sysbench is a scriptable multi-threaded benchmark tool based on LuaJIT.     
It is most frequently used for database benchmarks, but can also be used    
to create arbitrarily complex workloads that do not involve a database server.

## Install

```bash
wget 'http://imysql.com/wp-content/uploads/2014/09/sysbench-0.4.12-1.1.tgz'
tar -xvzf sysbench-0.4.12-1.1.tgz
cd sysbench-0.4.12-1.1
./autogen.sh
./configure --with-mysql-includes=/usr/local/mysql/include --with-mysql-libs=/usr/local/mysql/lib
make install
```

**NOTE**: This is just my installation method, the more ways to install you can get from [akopytov/sysbench](https://github.com/akopytov/sysbench).


## CPU test

```bash
$ sysbench --num-threads=2 --test=cpu --cpu-max-prime=20000 run

sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 2

Doing CPU performance benchmark

Threads started!
Done.

Maximum prime number checked in CPU test: 20000


Test execution summary:
    total time:                          12.8481s
    total number of events:              10000
    total time taken by event execution: 25.6933
    per-request statistics:
            min:                                  2.44ms
            avg:                                  2.57ms
            max:                                 31.09ms
            approx.  95 percentile:               2.88ms

Threads fairness:
    events (avg/stddev):           5000.0000/64.00
    execution time (avg/stddev):   12.8466/0.00
```

Lot of numbers, right? But all you need to is pay attention to `total time` item, compare it    
across multiple systems to know what these numbers are worth.

**NOTE**: Usually, you don't need to set `--num-threads` option, because this task is simple,   
it can be distributed evenly to each cpu thread. The total time will meet following equation:

```
   single thread total time
  -------------------------- = num threads
    num threads total time
```
