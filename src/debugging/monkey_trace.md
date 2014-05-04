# Monkey Trace

Monkey comes with an optional __Trace__ mode that can be only enabled on compile time. It aim to print to __STDOUT__ different trace messages across different sections of the core when running.

Due to it's verbosity this __Trace__ mode should only used under a development context as performance is affected. Never enable __Trace__ mode on production.

A printed trace line is formed using the following format:

    ~ seconds:milliseconds [component|file:line] function() message

Description of each field below:

|field | description|
|------|------------|
| seconds      |elapsed time in seconds since the server started   |
| milliseconds |number of milliseconds for the current seconds unit |
| component    |Monkey component, can be core or a plugin name |
| file         |Filename where the trace was generated |
| line         |Line of the file where the trace was generated |
| function     |Caller function() |
| message      |Trace message |

## Configuring

As said, Monkey needs to be recompiled with __Trace__ mode enabled, to accomplish that append the __--trace__ argument to the configure script:

```shell
$ ./configure --trace
```

Now is suggested to clean up any old object file and recompile:

```shell
$ make clean
$ make
```

## Usage

To verify that __Trace__ mode was enabled, start Monkey server and check the output printed to STDOUT:

```shell
$ bin/monkey
~  0.338244 [core|monkey.c:255] main() Monkey TRACE is enabled
Monkey HTTP Daemon 1.5.0
Built : May  4 2014 08:46:25 (gcc 4.8.2)
Home  : http://monkey-project.com
~  0.338750 [core|mk_vhost.c:394] mk_vhost_read() Map error page: status 404 -> 404.html
~  0.338836 [core|mk_file.c:100] mk_file_get_info() warning: target has not execution permission
~  0.339356 [core|mk_file.c:100] mk_file_get_info() warning: target has not execution permission
~  0.339618 [core|mk_plugin.c:500] mk_plugin_read_config() Load Plugin 'liana@/home/edsiper/coding/monkey/plugins/liana/monkey-liana.so'
~  0.339655 [core|mk_plugin.c:821] mk_plugin_preworker_calls() [liana] Set thread key
[+] Process ID is 6204
[+] Server socket listening on Port 2001
[+] 4 threads, 126 client connections per thread, total 504
[+] Transport layer by liana in http mode
[+] Linux Features: TCP_FASTOPEN SO_REUSEPORT
```

Now we can see in the top line after the server signature a message like this:

```
~  0.338244 [core|monkey.c:255] main() Monkey TRACE is enabled
```

Once the server start receiving requests the __Trace__ feature will print out messages about everything that is happening internally.

## Filtering

Sometimes when using __Trace__ mode we want to only trace specific files of Monkey core or some specific plugin, for this purpose exists a filtering mechanism that allows to just print desired events that happens on a specific file name.

A filter is set using the environment variable __MK_TRACE_FILTER__, many file names can be added. As an example, if we would just be interested into get trace messages from the Monkey Scheduler we should set the filter as follows:

```shell
$ export MK_TRACE_FILTER="mk_scheduler.c"
$ bin/monkey
```

As described, the filter must be set before to start Monkey. If you want to also trace multiple files just append them in the same filter line, e.g: if we are also interested into the Event Loop traces we should do:

```shell
$ export MK_TRACE_FILTER="mk_scheduler.c mk_epoll.c"
$ bin/monkey
```

Remember that __Monkey Trace__ should not be enabled on production, just for development purposes.
