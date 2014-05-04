# GNU Debugger

One of the most famous debuggers available on Linux and general Unix based systems is the GNU Debugger, well known as GDB. This section do not aim to explain how GDB works, but it specify the minimum requirements to make it work and be able to trace Monkey program properly.

If you want to get more details about GDB and it features, please refer to the official documentation from the GNU site:

http://www.gnu.org/software/gdb/documentation/

## Requirements

In order to use GDB, your Linux system must have the following tools:

* GCC or CLANG compiler
* GDB

Monkey must be compiled with debugging symbols, those can be enabled on __configure__ time appending the __--debug__ parameter, e.g:

```shell
$ ./configure --debug
```

after compile through __make__ command, the Monkey binary file will contain debugging symbols, you can do a simple check with the __file__ command:

```shell
$ file bin/monkey
bin/monkey: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=f15b588d793ddf095c294d418aa8c834e330f738, not stripped
```

the output of the __file__ command should indicate at the end __not stripped__.

## Testing Monkey with GDB

Running GDB is pretty straighforward, from the command line you need to use the __gdb__ program name and append the Monkey binary file:

```shell
$ gdb bin/monkey
GNU gdb (Ubuntu 7.7-0ubuntu3) 7.7
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

(gdb)
```

When reach the GDB prompt __(gdb)__, you can start running Monkey, or before that specify specific debugging instructions such as breakpoints.

On this simple test we will run Monkey under GDB and signal the Monkey process to simulate a segmentation fault, on that moment GDB will trap the exception and return back the control:

```shell
$ gdb bin/monkey
$ gdb bin/monkey
GNU gdb (Ubuntu 7.7-0ubuntu3) 7.7
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

(gdb)
```

Once in the GDB prompt, launch Monkey issuing the __run__ GDB command:

```shell
(gdb) run
Starting program: /home/edsiper/coding/monkey/bin/monkey
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Monkey HTTP Daemon 1.5.0
Built : May  3 2014 19:43:23 (gcc 4.8.2)
Home  : http://monkey-project.com
[New Thread 0x7ffff73ee700 (LWP 19097)]
[New Thread 0x7ffff6bed700 (LWP 19098)]
[New Thread 0x7ffff63ec700 (LWP 19099)]
[New Thread 0x7ffff5beb700 (LWP 19100)]
[New Thread 0x7ffff53ea700 (LWP 19101)]
[+] Process ID is 19093
[+] Server socket listening on Port 2001
[+] 4 threads, 126 client connections per thread, total 504
[+] Transport layer by liana in http mode
[+] Linux Features: TCP_FASTOPEN SO_REUSEPORT
```

You should see a similar output as the described above, the first step now would be to identify the Monkey process identificator or PID from the given output. On this case the PID is 19093:

```
[+] Process ID is 19093
```

From a different terminal we will signal the main process through the __kill__ command issuing a SIGSEGV signal to emulate a segmentation fault:

```shell
$ kill -SIGSEGV 19093
```

Going back to the terminal where GDB and Monkey are running, we catch the segmentation faul event:

```shell
Program received signal SIGSEGV, Segmentation fault.
0x00007ffff76b3d7d in nanosleep () at ../sysdeps/unix/syscall-template.S:81
../sysdeps/unix/syscall-template.S: No such file or directory.
(gdb)
```

If desired, the backtrace command through the __bt__ shortcut can be used in GDB to obtain a detailed trace of when the problem was faced:

```shell
(gdb) bt
#0  0x00007ffff76b3d7d in nanosleep () at ../sysdeps/unix/syscall-template.S:81
#1  0x00007ffff76b3c14 in __sleep (seconds=0) at ../sysdeps/unix/sysv/linux/sleep.c:137
#2  0x000000000041484e in mk_server_loop (server_fd=-1) at mk_server.c:90
#3  0x0000000000406bc3 in main (argc=1, argv=0x7fffffffdf08) at monkey.c:350
```

GDB can be used mostly when troubleshooting a Segmentation fault or tracing specific variables changes within others, as said, is suggested to refer to the official GDB documentation for more details.
