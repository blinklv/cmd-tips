# Static library and dynamic library

The following four files will be used to explain tips later.

**add.c**
```c
int add(int x, int y) {
  return x + y;
}
```
**sub.c**
```c
int sub(int x, int y) {
  return x - y;
}
```

**multi.c**
```c
int multi(int x, int y) {
  return x * y;
}
```

**divide.c**
```c
int divide(int x, int y) {
  return x/y;
}
```

## Static Library

### create static library

```bash
$ar rcs libcalc.a add.o sub.o
```

### get the file list of a archive file.

```bash
$ar t libcalc.a

add.o
sub.o
```

### add a object file to a existing archive file.

```bash
$ar rcs libcalc.a multi.o
$ar t libcalc.a

add.o
sub.o
multi.o

$ar rcs libcalc.a divide.o
$ar t libcalc.a

add.o
sub.o
multi.o
divide.o
```

### extract the object file(s) from a archive file.

```bash
$rm -f *.o
$ls

add.c  divide.c  libcalc.a  multi.c  sub.c

$ar x libcalc.a
$ls

add.c  add.o  divide.c  divide.o  libcalc.a  multi.c  multi.o  sub.c  sub.o
```

## Dynamic Library

### create dynamic (shared) library

```bash
$gcc -shared *.o -o libcalc.so
```

## Link

We need a extra file (**main.c**).

```c
#include<stdio.h>

int add(int x, int y);
int sub(int x, int y);
int multi(int x, int y);
int divide(int x, int y);

int main() {
  int result = divide(multi(sub(add(100, 20), 30), 20), 10);
  printf("%d\n", result);
}
```

### Link Dynamic Library

```bash
$ gcc main.c -L. -lcalc -o test
```

So, let's execute the **test** file.

```bash
$ ./test

./test: error while loading shared libraries: libcalc.so: cannot open shared object file: No such file or directory
```

It can't work, Why? Because the loader can't find the shared library, you need give the loader a   
little help.

**Using LD_LIBRARY_PATH environment variable**

```bash
$ env LD_LIBRARY_PATH="$(pwd):$LD_LIBRARY_PATH" ./test

180
```

**Specify -rpath option**

```bash
$ gcc main.c -Wl,-rpath="$(pwd)" -L. -lcalc -o test
$ ./test

180
```

### Link Static Library

**Specify the path of the static library**

```bash
$ gcc main.c ./libcalc.a -o test1
$ ./test1

180
```

**Add -static option**

```bash
$ gcc main.c -static -L. -lcalc -o test2
$ ./test2

180
```

There some differences between these two ways, we can use the **ldd** command to   
dig it. 

```bash
$ ldd ./test1

linux-vdso.so.1 =>  (0x00007fffa25fe000)
/$LIB/libonion.so => /lib64/libonion.so (0x00007f4e2d30f000)
libc.so.6 => /lib64/libc.so.6 (0x00007f4e2cf67000)
libdl.so.2 => /lib64/libdl.so.2 (0x00007f4e2cd63000)
libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f4e2cb46000)
/lib64/ld-linux-x86-64.so.2 (0x00007f4e2d41f000)

$ ldd ./test2

not a dynamic executable
```

If we give the path of the static library to gcc, it only means this static library will be linked,  
but other libraries will also be linked in dynamic mode. If we specify the **-static** option,   
it will prevent linking with the shared library.
