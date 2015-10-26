# Build and Installation

Start with Monkey is pretty easy, you don't need to be a privileged system user, you can run it locally or install it in a generic path.

## Requirements

Monkey uses very low CPU and Memory consumption, it works fine on any x86, x86_64 platform without problems.

Regarding to the software required to run Monkey successfully, if you have a Linux box, it will require a Kernel version 2.6.32 (at least), for OSX it needs at least Yosemite. Besides that, the core of Monkey just requires two basic libraries: pthreads and libc, if you use Linux, we are 99% sure that you should not care about this requirement.

## Build System

Before to proceed to build Monkey, you have be aware that the build system is based on CMake and we _optionally_ provides a bash script called _configure_ that works as a wrapper for who's not familiar with CMake.

Below you will find instructions and options available at our configure script, if you want to use the CMake way, please make sure to read the [Monkey + CMake](cmake_options.md) documentation for further explanations of the options available.


## Download

Monkey is mainly distributed in a compressed tarball which contains the full source code, the available releases can be found in the official site http://monkey-project.com. At the moment of this document is written, the 1.6.x series is available. You can find more details in the [downloads](http://monkey-project.com/downloads) section.

## Configure and compile

Once you get the tarball you can use the following simple steps to configure, compile and test Monkey locally:

```shell
$ tar zxfv monkey-1.6.0.tar.gz
$ cd monkey-1.6.0
$ ./configure --local
$ make
```

The _--local_ option instruct the build system to operate locally without the need of installing something outside of the current path (_make install_ is __not__ necessary). If you want to proceed with common options for installing, do not use that feautre and pay attention to the other features listed below.

## Specific configure options

Monkey v1.6 build system is based on CMake and provides a _configure_ that acts as a wrapper over the CMake options available. In order to get a full list of options available you can run './configure -h', e.g:

```bash
$ ./configure -h
Usage: ./configure [OPTION]... [VAR=VALUE]...

Optional Commands:
  --help        Display this help and exit
  --version     Display version information and exit

Build options:
  --local                 Build locally, do not install (dev mode)
  --debug                 Compile Monkey with debugging symbols
  --trace                 Enable trace messages (do not use in production)
  --no-backtrace          Disable backtrace feature
  --linux-trace           Enable Linux Trace Toolkit
  --musl-mode             Enable musl compatibility mode
  --uclib-mode            Enable uClib compatibility mode
  --malloc-libc           Use system default memory allocator (default is jemalloc)
  --pthread-tls           Use Posix thread keys instead of compiler TLS
  --no-binary             Do not build binary
  --static-lib-mode       Build static library mode
  --skip-config           Do not include configuration files

Installation Directories:
  --prefix=PREFIX         Root prefix directory
  --sbindir=BINDIR        Binary files (executables)
  --libdir=LIBDIR         Libraries
  --includedir=INCDIR     Header install path
  --sysconfdir=SYSCONFDIR Configuration files
  --webroot=WEB_ROOT      Path to default web site files
  --mandir=MANDIR         Manpages - documentation
  --logdir=LOGDIR         Log files
  --pidfile=PIDFILE       Path to file to store PID
  --systemddir[=DIR]      Systemd directory path
  --enable-plugins=a,b    Enable the listed plugins
  --disable-plugins=a,b   Disable the listed plugins
  --static-plugins=a,b    Build plugins in static mode
  --only-accept           Use only accept(2)
  --only-accept4          Use only accept4(2) (default and preferred)

Override Server Configuration:
  --default-port=PORT     Override default TCP port (default: 2001)
  --default-user=USER     Override default web user (default: www-data)
```

## Running Monkey

If the compilation process has ended successfully, you are ready to run Monkey Web Server executing the following command:

```bash
$ build/monkey
```

once the server has started up, you should get something like this in your console:

```bash
$ build/monkey
Monkey HTTP Server v1.6.0
Built : Jul 31 2015 16:16:37 (/usr/bin/cc 4.9.2)
Home  : http://monkey-project.com
[+] Process ID is 9300
[+] Server listening on 0.0.0.0:2001
[+] 4 threads, may handle up to 1024 client connections
[+] Loaded Plugins: liana
[+] Linux Features: TCP_FASTOPEN SO_REUSEPORT
[2015/07/31 16:16:39] [   Info] HTTP Server started
```

by default, Monkey listen for incomming connections on TCP port 2001, give it a try using your favorite web browser at http://127.0.0.1:2001
