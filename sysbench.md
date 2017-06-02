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

A lot of numbers, right? But all you need to is pay attention to `total time` item, compare it    
across multiple systems to know what these numbers are worth.

**NOTE**: Usually, you don't need to set `--num-threads` option, because this task is simple,   
it can be distributed evenly to each cpu thread. The total time will meet following equation:

```
   single thread total time
  -------------------------- = num threads
    num threads total time
```


## mysql OLTP (on-line transaction processing) test

The first step is to use the **prepare** statement with **sysbench** to generate a table in the    
specified database which will be used when performing tests.

```bash
$ sysbench --test=oltp --oltp-table-size=1000000  --db-driver=mysql \
         --mysql-host=10.240.64.138 --mysql-port=3814 --mysql-db=kt_pay_stat --mysql-user=kt_pay_stat --mysql-password=5ca4932e3 prepare

sysbench 0.4.12:  multi-threaded system evaluation benchmark

Creating table 'sbtest'...
Creating 1000000 records in table 'sbtest'...
```

The second step, run a simple read/write test.

```bash
$ sysbench --num-threads=4 --max-time=120 --max-requests=0 \
           --test=oltp --oltp-test-mode=complex --oltp-read-only=off --oltp-table-size=1000000 --oltp-dist-type=uniform --db-driver=mysql \
           --mysql-host=10.240.64.138 --mysql-port=3814 --mysql-db=kt_pay_stat --mysql-user=kt_pay_stat --mysql-password=5ca4932e3 run

sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 4

Doing OLTP test.
Running mixed OLTP test
Using Uniform distribution
Using "BEGIN" for starting transactions
Using auto_inc on the id column
Threads started!
Time limit exceeded, exiting...
(last message repeated 3 times)
Done.

OLTP test statistics:
    queries performed:
        read:                            422436
        write:                           150870
        other:                           60348
        total:                           633654
    transactions:                        30174  (251.44 per sec.)
    deadlocks:                           0      (0.00 per sec.)
    read/write requests:                 573306 (4777.30 per sec.)
    other operations:                    60348  (502.87 per sec.)

Test execution summary:
    total time:                          120.0062s
    total number of events:              30174
    total time taken by event execution: 479.8604
    per-request statistics:
          min:                                 13.70ms
          avg:                                 15.90ms
          max:                                 64.98ms
          approx.  95 percentile:              17.56ms

Threads fairness:
    events (avg/stddev):           7543.5000/516.70
    execution time (avg/stddev):   119.9651/0.00
```

I like paying attention to `max` and `approx` item. Usually, the test duration (`--max-time` option) should    
be larger than 1 hour in the production environment (I just write this tip, so set it too small). There are    
so many alternative options, and you can combine them to simulate the real scenario. 

Cleanup the test data in the last step.

```bash
$ sysbench --test=oltp --db-driver=mysql \
           --mysql-host=10.240.64.138 --mysql-port=3814 --mysql-db=kt_pay_stat --mysql-user=kt_pay_stat --mysql-password=5ca4932e3 cleanup

sysbench 0.4.12:  multi-threaded system evaluation benchmark

Dropping table 'sbtest'...
Done.
```
