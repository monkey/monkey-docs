# Installation

Start with Monkey is pretty easy, you don't need to be a privileged system user, you can run it locally or install it in a generic path.

## Requirements

Monkey uses very low CPU and Memory consumption, it works fine on any x86, x86_64 platform without problems.

Regarding to the software required to run Monkey successfully, you will need a Linux environment with Kernel version 2.6.29 (at least). The server is developed on top of the Kernel features, so use an older version can cause some buggy behaviors.

Besides the kernel version, the core of Monkey just requires two basic libraries: pthreads and libc, if you use Linux, we are 99% sure that you should not care about this requirement.

## Download

Monkey is mainly distributed in a compressed tarball which contains the full source code, the available releases can be found in the official site http://monkey-project.com. At the moment of this document is written, the 1.0.0 version is available. You can find more details in the Downloads section.

## Configure and compile

Once you get the tarball you can use the following simple steps to configure and compile the source code:

```shell
$ tar zxfv monkey-1.5.0.tar.gz
$ cd monkey-1.4.0
$ ./configure
$ make
```

## Specific configure options

Monkey uses a similar autotools scripts to configure and set the proper enviroment. Also, you can configure Monkey with your specific paths, just run './configure -h' to get a full list of the available options as:

```bash
$ ./configure --help
Usage: ./configure [OPTION]... [VAR=VALUE]...

Optional Commands:
  --help        Display this help and exit
  --version     Display version information and exit

Compiler and debug Features:
  --debug                  Compile Monkey with debugging symbols
  --trace                  Enable trace messages (do not use in production)
  --no-backtrace           Disable backtrace feature
  --linux-trace            Enable Linux Trace Toolkit
  --musl-mode              Enable musl compatibility mode
  --uclib-mode             Enable uClib compatibility mode
  --malloc-libc            Use system default memory allocator (default is jemalloc)
  --platform=PLATFORM      Target platform: 'generic' or 'android' (default: generic)

Shared library options:
  --enable-shared          Build Monkey as a shared library in addition to a stand-alone server
  --enable-relaxed-plugins Allow the application to make the library load arbitrary plugins.
                           WARNING security risk, do not enable in distro packages!

Installation Directories:
  --prefix=PREFIX         Root prefix directory
  --bindir=BINDIR         Binary files (executables)
  --libdir=LIBDIR         Libraries
  --incdir=INCDIR         Header install path
  --sysconfdir=SYSCONFDIR Configuration files
  --datadir=DATADIR       Specific Monkey data files
  --mandir=MANDIR         Manpages - documentation
  --logdir=LOGDIR         Log files
  --plugdir=PLUGDIR       Plugins directory path
  --systemddir[=DIR]      Systemd directory path
  --enable-plugins=a,b    Enable the listed plugins
  --disable-plugins=a,b   Disable the listed plugins
  --only-accept           Use only accept(2)
  --only-accept4          Use only accept4(2) (default and preferred)
  --safe-free             Force Monkey to free resources before exit

Override Server Configuration:
  --default-port=PORT     Override default TCP port (default: 2001)
  --default-user=USER     Override default web user (default: nobody)
```


## Running Monkey web server

Monkey configuration system is designed to let run the service from the local directory without an installation process unless you specify an installation directory in the configuration step using the --prefix option.

If the compilation process has ended successfully, you are ready to run Monkey Web Server executing the following command:

```
$ bin/monkey
```

once the server has started up, you should get something like this in your console:

```
$ bin/monkey
Monkey HTTP Daemon 1.5.0
Built : Apr 30 2014 22:08:39 (gcc 4.8.2)
Home  : http://monkey-project.com
[+] Process ID is 32659
[+] Server socket listening on Port 2001
[+] 1 threads, 508 client connections per thread, total 508
[+] Transport layer by liana in http mode
[+] Linux Features: TCP_FASTOPEN SO_REUSEPORT TCP_AUTOCORKING
```

by default, Monkey listen for incomming connections on TCP port 2001, give it a try using your favorite web browser at http://127.0.0.1:2001
