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
