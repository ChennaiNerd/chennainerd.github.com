---
layout: post
title: "GCOV - C/C++ Code coverage testing tool"
date: 2014-06-08 11:39:18 +0530
comments: true
author: Yogeswaran
categories: ["code coverage", "c", "c++"]
---

## What is GCOV

- GCC provides GCOV, code coverage testing tool for C/C++ programs.
- GCOV identifies the lines of code that got executed while running the program.
- It also gives additional information like how many times particular line got executed.
- Also provides information about how many possible branches are there in the code and which branch path got executed more.

## Use cases

### Optimization

GCOV identifies the sections in the code that are heavy executed,
using which we'll be able to **focus on optimizing the parts of the code
which are executed often**.

### Identifying dead code

Any code that got compiled but never got executed on any possible scenario can be
found using GCOV.  Removing such code can help in **reducing the memory footprint**
of the program.  This can be vital information for programs running on embedded platforms.

### Reliability of testing

The coverage report can help in **identifying the gaps in testing**.  
The coverage information can be used for writing test cases to exercise
the uncovered area in the code.

## Instrumenting GCOV

GCOV does not require any change in the code. The only requirement is to have the code built with **-fprofile-arcs** and
**-ftest-coverage**  compiler and linker flags.

```
yogi@u32:~/gcov_basics$ ls -l
total 8
-rwxrwx--- 1 yogi vboxsf 131 Jun  7 15:13 coverage.c
-rwxrwx--- 1 yogi vboxsf 286 Jun  7 15:56 Makefile
yogi@u32:~/gcov_basics$ cat coverage.c
#include <stdio.h>
int main(int argc, char* argv[])
{
  if (argc == 1)
    printf("True\n");
  else
    printf("False\n");
  return 0;
}
yogi@u32:~/gcov_basics$ cat Makefile
CC=gcc
CFLAGS=-fprofile-arcs -ftest-coverage
LDFLAGS=-fprofile-arcs -ftest-coverage
TARGET=cov
SRC=coverage.c

all:	obj
  $(CC) $(LDFLAGS) *.o -o $(TARGET)
obj:
  $(CC) $(CFLAGS) -c $(SRC)
clean:
  rm -f $(TARGET) *.html *.gc* *.o
gcov:
  gcovr -r . --html -o coverage.html --html-details
```

>CFLAGS += -fprofile-arcs -ftest-coverage

`CFLAGS` are meant to be used during compilation. This will create `.gcno` file corresponding to `.c/.cpp` file

```
yogi@u32:~/gcov_basics$ make obj
gcc -fprofile-arcs -ftest-coverage -c coverage.c
yogi@u32:~/gcov_basics$ ls -l
total 16
-rwxrwx--- 1 yogi vboxsf  131 Jun  7 15:13 coverage.c
-rw-rw-r-- 1 yogi yogi    396 Jun  7 15:56 coverage.gcno
-rw-rw-r-- 1 yogi yogi   1824 Jun  7 15:56 coverage.o
-rwxrwx--- 1 yogi vboxsf  286 Jun  7 15:56 Makefile
```

>LDFLAGS += -fprofile-arcs -ftest-coverage

`LDFLAGS` are meant to be used during linking.  

```
yogi@u32:~/gcov_basics$ make
gcc -fprofile-arcs -ftest-coverage -c coverage.c
gcc -fprofile-arcs -ftest-coverage *.o -o cov
yogi@u32:~/gcov_basics$ ls -l
total 36
-rwxrwxr-x 1 yogi yogi   17295 Jun  7 15:56 cov
-rwxrwx--- 1 yogi vboxsf   131 Jun  7 15:13 coverage.c
-rw-rw-r-- 1 yogi yogi     396 Jun  7 15:56 coverage.gcno
-rw-rw-r-- 1 yogi yogi    1824 Jun  7 15:56 coverage.o
-rwxrwx--- 1 yogi vboxsf   286 Jun  7 15:56 Makefile
```

```
yogi@u32:~/gcov_basics$ ./cov
True
yogi@u32:~/gcov_basics$ ls -l
total 40
-rwxrwxr-x 1 yogi yogi   17295 Jun  7 15:56 cov
-rwxrwx--- 1 yogi vboxsf   131 Jun  7 15:13 coverage.c
-rw-rw-r-- 1 yogi yogi     160 Jun  7 15:56 coverage.gcda
-rw-rw-r-- 1 yogi yogi     396 Jun  7 15:56 coverage.gcno
-rw-rw-r-- 1 yogi yogi    1824 Jun  7 15:56 coverage.o
-rwxrwx--- 1 yogi vboxsf   286 Jun  7 15:56 Makefile
```

`.gcno` has static information about the file.

`.gcda` has dynamic information about the file based on the path taken during execution.

`.gcno` and .gcda files together are required to generate the coverage report.

## Generating Report

Either **gcov** or **gcovr** can be used for generating coverage report.

**gcov** utility will be installed as part of **gcc** in most of the Linux distributions.

```
yogi@u32:~/gcov_basics$ gcov coverage.c
File 'coverage.c'
Lines executed:80.00% of 5
coverage.c:creating 'coverage.c.gcov'

yogi@u32:~/gcov_basics$ cat coverage.c.gcov
        -:    0:Source:coverage.c
        -:    0:Graph:coverage.gcno
        -:    0:Data:coverage.gcda
        -:    0:Runs:1
        -:    0:Programs:1
        -:    1:#include <stdio.h>
        1:    2:int main(int argc, char* argv[])
        -:    3:{
        1:    4:	if (argc == 1)
        1:    5:		printf("True\n");
        -:    6:	else
    #####:    7:		printf("False\n");
        1:    8:	return 0;
        -:    9:}

```

```
Legend:
  -       indicates not an executable statement
  #####   indicates statement the did not get executed
  1       (or any number) indicates the number of times the statement got executed.
```

**gcovr** is a python utility on top of gcov.  It can be installed using pip.

```
$ pip install gcovr
```

```
yogi@u32:~/gcov_basics$ gcovr -r . --html -o coverage.html --html-details
```

The above command will generate coverage report in html format.

## Cool features

- GCOV takes care of **conditional compilation**.  If a file has 100 lines of code but only 50 lines of code got conditionally compiled using ifdef, then, only 50 lines is taken into account for calculating the code coverage.

- GCOV when enabled on **shared library** and called from two different applications, will consolidate the coverage based on execution of both the applications.

- GCOV works **across reboot**.  The execution information can be collected and consolidated across reboot.

- Running the **same executable multiple instances**, appends execution information to .gcda file.

- Coverage can be **collected from different physical** machines by copying the executable and .gcda files.

## Pointer to ponder

- The **number of lines in the file does not match exactly with the number of lines considered for testing coverage**.  One reason is, **not all lines are statement** to be executed. Say, { in a line is not a candidate for execution. The next is could be due to **compiler optimization**.

- The **version of .gcno file and .gcda file should exactly match** to generate report.  If the code was compiled again even without any change, report will not get generated as there is mismatch in the version of .gcno and .gcda files.

- If .gcno file was created again even without changing anything in the code it will not match with .gcda file.

- The program should **gracefully exit** to create/append-to .gcda file.  

- If the program is a daemon, better to add **exit(0) in the SIGINT and SIGTERM signal handler.** to do a graceful exit.

- GCOV will try to create **.gcda file in the same folder structure as it was compiled**.
But the problem could be when used on embedded platforms, where the filesystem is mostly readonly.
In this case, **GCOV_PREFIX** enviromental variable can be used.

```
yogi@u32:~/gcov_basics$ make
gcc -fprofile-arcs -ftest-coverage -c coverage.c
gcc -fprofile-arcs -ftest-coverage *.o -o cov

Note: Copying the executable to /tmp folder

yogi@u32:~/gcov_basics$ cp cov /tmp/
yogi@u32:~/gcov_basics$ ls -l
total 36
-rwxrwxr-x 1 yogi yogi   17295 Jun  8 07:30 cov
-rwxrwx--- 1 yogi vboxsf   131 Jun  7 15:13 coverage.c
-rw-rw-r-- 1 yogi yogi     396 Jun  8 07:30 coverage.gcno
-rw-rw-r-- 1 yogi yogi    1824 Jun  8 07:30 coverage.o
-rwxrwx--- 1 yogi vboxsf   286 Jun  7 16:32 Makefile
yogi@u32:~/gcov_basics$ cd /tmp/
yogi@u32:/tmp$ ./cov
True
yogi@u32:/tmp$ find . -name "*.gcda"

Note: gcda file will not get created in the current working directory, instead
will be created in the same folder structure as it got compiled.

yogi@u32:/tmp$ cd -
/home/yogi/gcov_basics

Note: gcda file getting created where the code was actually compiled.

yogi@u32:~/gcov_basics$ ls -l
total 40
-rwxrwxr-x 1 yogi yogi   17295 Jun  8 07:30 cov
-rwxrwx--- 1 yogi vboxsf   131 Jun  7 15:13 coverage.c
-rw-rw-r-- 1 yogi yogi     160 Jun  8 07:30 coverage.gcda
-rw-rw-r-- 1 yogi yogi     396 Jun  8 07:30 coverage.gcno
-rw-rw-r-- 1 yogi yogi    1824 Jun  8 07:30 coverage.o
-rwxrwx--- 1 yogi vboxsf   286 Jun  7 16:32 Makefile
yogi@u32:~/gcov_basics$
yogi@u32:~/gcov_basics$ export GCOV_PREFIX=/tmp
yogi@u32:~/gcov_basics$ rm coverage.gcda
yogi@u32:~/gcov_basics$ cd -
/tmp
yogi@u32:/tmp$ ./cov
True

Note: By setting GCOV_PREFIX environmental variable we'll be able to direct
the files to a particular base folder.

yogi@u32:/tmp$ ls -l /tmp/home/yogi/gcov_basics/coverage.gcda
-rw-rw-r-- 1 yogi yogi 160 Jun  8 07:31 /tmp/home/yogi/gcov_basics/coverage.gcda
```

- **GCOV_PREFIX_STRIP** environmental variable can be handy when we are not interested in complete folder structure but to remove certain part of it.

```
yogi@u32:/tmp$ export GCOV_PREFIX=/tmp
yogi@u32:/tmp$ export GCOV_PREFIX_STRIP=2
yogi@u32:/tmp$ ./cov
True

Note:  Earlier .gcda file was getting created in /tmp/home/yogi/gcov_basics/ folder.
Now by exporting GCOV_PREFIX_STRIP=2 environmental variable, will strip two levels - /home/yogi/ folder
is stripped off and .gcda file will get create in /tmp/gcov_basics/

yogi@u32:/tmp$ ls -l /tmp/gcov_basics/coverage.gcda
-rw-rw-r-- 1 yogi yogi 160 Jun  8 07:44 /tmp/gcov_basics/coverage.gcda

```

## FAQs

1.undefined reference to `__gcov_init`

```
yogi@u32:~/gcov_basics$ make
gcc -fprofile-arcs -ftest-coverage -c coverage.c
gcc  *.o -o cov
coverage.o: In function `_GLOBAL__sub_I_65535_0_main':
coverage.c:(.text+0xae): undefined reference to `__gcov_init'
coverage.o:(.data+0x24): undefined reference to `__gcov_merge_add'
collect2: ld returned 1 exit status
make: *** [all] Error 1
```

>The reason for this is, **-fprofile-arcs** and **-ftest-coverage** where used during compilation(in CFLAGS), but missed during linking (in LDFLAGS).

2.`.gcda` file not getting created as the result of execution.

> Check if gcov symbols are there in the binary using **strings** or **nm** command.

>**ldd** command **will not help** because there will not be any extra libraries linked specifically for gcov.

Binary **without gcov symbols** will look like the one shown below.

```
yogi@u32:~/gcov_basics$ strings cov
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
puts
__libc_start_main
GLIBC_2.0
PTRh
UWVS
[^_]
True
False
;*2$"
```

Binary **with gcov symbols** will look like the one shown below.

```
yogi@u32:~/gcov_basics$ strings cov
/lib/ld-linux.so.2
__gmon_start__
libc.so.6
_IO_stdin_used
...
[^_]
True
False
/home/yogi/gcov_basics/coverage.gcda
profiling:%s:Version mismatch - expected %.4s got %.4s
profiling:%s:Overflow merging
profiling:%s:Overflow writing
profiling:%s:Cannot create directory
profiling:%s:Not a gcov data file
profiling:%s:Merge mismatch for %s
profiling:%s:Invocation mismatch - some data files may have been removed%s
function
summaries
profiling:%s:Error merging
profiling:%s:Error writing
GCOV_PREFIX_STRIP
GCOV_PREFIX
profiling:%s:Skip
profiling:%s:Cannot open
...
```
> The reason for this could be **-fprofile-arcs and -ftest-coverage CFLAGS** were missed during compilation.

3.gcov symbols are seen in the binary but still .gcda file is not getting created.

> The reason could be the program did not do a graceful exit.


 We’d love to hear more about your experiences with c/c++ code coverage.
 Please share your thoughts in the comments below.
