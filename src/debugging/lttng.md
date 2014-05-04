# Linux Trace Toolkit

The [Linux Trace Toolkit or LTTng)](https://lttng.org/ust) on it's user space version, is a third party software component that aims to provide a highly efficient tracing tools for Linux. Its tracers help tracking down performance issues and debugging problems involving multiple concurrent processes and threads.

Starting from [Monkey v1.4](http://monkey-project.com/Announcements/v1.4.0), LTTng is integrated as an optional debugging feature that can be enabled on configure/compile time. One of the major goals of this integration is the capacity to trace different Monkey core events with specific variables and values at certain levels of work.

## Requirements

[LTTng](http://lttng.org/ust) software is distributed among major Linux distributions but is important to make sure the software installed have the full components required by Monkey:

* LTTng-ust library
* LTTng-ust headers
* LTTng tools

On Debian/Ubuntu systems, the packages can be installed with the following command:

```shell
$ sudo apt-get install lttng-tools liblttng-ust0 liblttng-ust-dev
```

Once the dependencies are installed, Monkey can be configured appending the __--linux-trace__ argument to the configure script:

```shell
$ ./configure --linux-trace
```

then make sure to cleanup any old object symbols and perform a clean build:

```shell
$ make clean
$ make
```

## Usage

When Monkey is running, it contain certain LTTng tracepoints that were enabled by the previous configuration made, those tracepoint will only act if a __LTTng Session Daemon__ have been created. A LTTng session can be created/destroyed any time without affect the running program, so is pretty safe to have a Monkey instance compiled with LTTng support and enable debugging any time is required without affect performance.

Monkey defines the following events that can be traced:

* Scheduler
* Event Loop events
* Event Loop socket states

Before to start tracing, make sure the LTTng session exists, otherwise a new one needs to be created, the common steps to accomplish this are:

```shell
$ lttng create monkey
$ lttng enable-event -u -a
```

Now a LTTng session called __monkey__ exists, you can verify this with the __list__ command:

```shell
$ lttng list monkey
Tracing session monkey: [inactive]
    Trace path: /home/foo/lttng-traces/monkey-20140503-212844

=== Domain: UST global ===

Buffer type: per UID

Channels:
-------------
- channel0: [enabled]

    Attributes:
          overwrite mode: 0
          subbufers size: 131072
          number of subbufers: 4
          switch timer interval: 0
          read timer interval: 0
          output: mmap()

    Events:
          * (type: tracepoint) [enabled]
```

The session exists but is __inactive__, note that LTTng trace will only work in active tracing sessions, to enable the tracing issue the __start__ command:

```shell
$ lttng start
Tracing started for session monkey
```

Now everything is in place: Monkey is running, LTTng session created and LTTng traces for __monkey__ session are active. Now the goal would be to issue some HTTP request to the server to make it generate some events. From the same terminal issue a request with the __curl__ client:

```shell
$ curl --head http://localhost:2001/
HTTP/1.1 200 OK
Server: Monkey/1.5.0
Date: Sun, 04 May 2014 03:39:43 GMT
Last-Modified: Tue, 04 Feb 2014 14:02:21 GMT
Content-Type: text/html
Content-Length: 6794
```

We have gathered some events into Monkey, now it's time to stop the LTTng tracing:

```shell
$ lttng stop
Waiting for data availability
Tracing stopped for session monkey
```

LTTng restriction is that you cannot view results until the trace have been stopped, if the stop command have ran successfully, issue the __view__ command:

```shell
$ lttng view
Trace directory: /home/foo/lttng-traces/monkey-20140503-214017

[21:41:07.294580765] mk:epoll: { cpu_id = 3 }, { fd = 0, event = "TIMEOUT CHECK" }
[21:41:07.295296494] epoll: { cpu_id = 1 }, { fd = 0, event = "TIMEOUT CHECK" }
[21:41:07.297864663] epoll: { cpu_id = 1 }, { fd = 0, event = "TIMEOUT CHECK" }
[21:41:07.303668362] epoll: { cpu_id = 3 }, { fd = 0, event = "TIMEOUT CHECK" }
[21:41:15.353402034] epoll: { cpu_id = 0 }, { fd = 13, event = "EPOLLIN" }
[21:41:15.353431391] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET: NOT FOUND" }
[21:41:15.353433848] epoll_state: { cpu_id = 0 }, { fd = 29, event = "SET: NEW" }
[21:41:15.353435214] scheduler: { cpu_id = 0 }, { fd = 29, event = "ADD_CLIENT_REUSEPORT" }
[21:41:15.353437571] scheduler: { cpu_id = 0 }, { fd = 29, event = "REGISTERED" }
[21:41:15.353438834] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.353447409] scheduler: { cpu_id = 0 }, { fd = 29, event = "GET_CONNECTION" }
[21:41:15.353448616] scheduler: { cpu_id = 0 }, { fd = 29, event = "GET_CONNECTION" }
[21:41:15.353475088] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.353476215] epoll_state: { cpu_id = 0 }, { fd = 29, event = "SET: CHANGE" }
[21:41:15.353480324] epoll: { cpu_id = 0 }, { fd = 29, event = "EPOLLOUT" }
[21:41:15.353481322] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.353482555] scheduler: { cpu_id = 0 }, { fd = 29, event = "GET_CONNECTION" }
[21:41:15.353483445] scheduler: { cpu_id = 0 }, { fd = 29, event = "GET_CONNECTION" }
[21:41:15.353661461] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.353663419] epoll_state: { cpu_id = 0 }, { fd = 29, event = "SET: CHANGE" }
[21:41:15.559031145] epoll: { cpu_id = 0 }, { fd = 29, event = "EPOLLIN" }
[21:41:15.559038475] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.559068884] epoll: { cpu_id = 0 }, { fd = 29, event = "FORCED CLOSE" }
[21:41:15.559091402] epoll_state: { cpu_id = 0 }, { fd = 29, event = "GET" }
[21:41:15.559094750] epoll_state: { cpu_id = 0 }, { fd = 29, event = "DELETE" }
[21:41:15.559098662] scheduler: { cpu_id = 0 }, { fd = 29, event = "GET_CONNECTION" }
[21:41:15.559269061] scheduler: { cpu_id = 0 }, { fd = 29, event = "DELETE_CLIENT" }
```
> This output have been filtered to avoid long lines in the page.

Using LTTng in Monkey is very useful to understand internals and trace possible issues when running in production. The performance penalty using LTTng is very low and is a huge win compared to a common downtime and troubleshooting session.
