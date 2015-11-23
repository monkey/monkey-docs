# CMake Options

[Monkey](http://monkey-project.com) build system is based on [CMake](http://www.cmake.org), despites we provide a wrapper bash script called _configure_, you may want to build using CMake directly, on this section you will find the options available.

## Getting Started with CMake

We suggest that you always use CMake command line inside the _build/_ directory located on the Monkey sources, to build it with default options just do:

```bash
$ cmake ../
```

If you want to turn on/off some of the options, you need to use the _-D_ argument plus the option with the new value. As an example, if you want to disable the Backtrace support the following command will do it:

```bash
$ cmake -DWITH_BACKTRACE=OFF ../
```

> most of the options are boolean, for hence they accept values ON, OFF, YES or NO.

If you are testing and building multiple times, CMake creates a cache of your setup and sometimes you will find your options are not being considered, we recommend that you remove all content inside the _build/_ directory that you are using to avoid problems.

## General Options

| option            |  description                               | default  |
|-------------------|--------------------------------------------|----------|
| BUILD_LOCAL       | Build Monkey locally, no install required  | No       |
| WITH_DEBUG        | Build with debug symbols                   | No       |
| WITH_ACCEPT       | Use accept(2) system call                  | No       |
| WITH_ACCEPT4      | Use accept4(2) system call                 | Yes      |
| WITH_LINUX_KQUEUE | Use Linux kqueue emulator                  | No       |
| WITH_TRACE        | Enable Trace mode                          | No       |
| WITH_UCLIB        | Enable uClib libc support                  | No       |
| WITH_MUSL         | Enable Musl libc support                   | No       |
| WITH_BACKTRACE    | Enable backtrace feature                   | Yes      |
| WITH_LINUX_TRACE  | Enable Lttng support                       | No       |
| WITH_PTHREAD_TLS  | Use old Pthread TLS mode                   | No       |
| WITH_SYSTEM_MALLOC| Use default system memory allocator (default: built-in Jemalloc) | No       |
| WITH_MBEDTLS_SHARED| Use mbedTLS as shared library             | No       |

## Plugins

Here is the list of options for each plugins and it default behavior for the build system. Note that when building plugins, they can be build in two modes: static or shared. Plugins that are build in static mode are build-together with the target binary, when shared mode is used, the plugins are created as a shared library that are loaded everytime Monkey starts.

| option                | name | description                         | build | mode |
|-----------------------|--------------------------------------------|-------|------|
| WITH_PLUGIN_AUTH      | auth | Enable Basic Authentication         | Yes   | shared|
| WITH_PLUGIN_CGI       | cgi  | CGI support                         | Yes   | shared|
| WITH_PLUGIN_CHEETAH   | cheetah| Cheetah Shell Interface           | Yes   | shared|
| WITH_PLUGIN_DIRLISTING| dirlisting| Directory Listing              | Yes   | shared|
| WITH_PLUGIN_FASTCGI   | fastcgi | FastCGI support                  | Yes   | shared|
| WITH_PLUGIN_LIANA     | liana | Basic Network Layer                | Yes   | __static__|
| WITH_PLUGIN_LOGGER    | logger | Log Writer                        | Yes   | shared|
| WITH_PLUGIN_MANDRIL   | mandril | Security                         | Yes   | shared |
| WITH_PLUGIN_TLS       | tls | TLS/SSL support                      | No    | __static__|

For more details about loading plugins built in shared mode, please refer to the [Plugins](../configuration/plugins.md) section.

## Extra build options

| option                | description                       | default |
|-----------------------|-----------------------------------|---------|
| WITHOUT_BIN           | Do not build Monkey binary        | No      |
| WITHOUT_CONF          | Skip configuration files          | No      |
| WITH_STATIC_LIB_MODE  | Build Monkey as a static library  | No      |
| WITH_PLUGINS          | Define a list of plugins to be included in the build phase. This variable accept the plugins name separated by a coma (__,__): -DWITH_PLUGINS=tls,fastcgi| N/A |
| WITHOUT_PLUGINS       | Define a list of plugins that __must not__ be included in the build phase. This variable accept the plugins name separated by a coma (__,__): -DWITHOUT_PLUGINS=cgi,fastcgi| N/A |
| STATIC_PLUGINS        | Define a list of enabled plugins that will be build in static mode. This variable accept the plugins name separated by a coma (__,__): -DSTATIC_PLUGINS=liana,tls | liana |
| SKIP_EMPTY_DIRS       | When installing, do not create the empty directories for the PID and logs | No |
| DEFAULT_PORT          | Set the default TCP port | 2001 |
| DEFAULT_USER          | Set the default user which Monkey must switch to when running as root | www-data |


## Install path options

The following options affects the setup for the file system paths when installing Monkey:

| option                | description                       |
|-----------------------|-----------------------------------|
| CMAKE_INSTALL_PREFIX  | Installation prefix path          |
| CMAKE_INSTALL_SBINDIR | Binary dir target path            |
| CMAKE_INSTALL_MANDIR  | Man pages path                    |
| INSTALL_SYSCONFDIR    | Configuration files path          |
| INSTALL_WEBROOTDIR    | Default HTML content path         |
| INSTALL_INCLUDEDIR    | Development Header files path     |
| INSTALL_LOGDIR        | Log directory path                |
| PID_PATH              | Target path to store the pid file |
| PID_FILE              | Name of the pid file              |
| SYSTEMD_DIR           | Absolute path to systemd script   |
