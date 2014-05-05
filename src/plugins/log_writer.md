# Log Writer - Logger

The __Log Writer__ plugin or __Logger__ for short, allows the creation of log files to store server events and register the status of each incoming HTTP request. It's designed to work with an in-memory buffer allowing flushing back the data to the file system at certain intervals of time.


## Enable Plugin

To enable the __Logger__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-logger.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

The plugin allows to register events at two scopes:

| scope | description |
|-------|-------------|
| global       | Defines the general behavior of the Plugin. |
| virtual host | Specific Log rules for the [Virtual Host](../virtualhost/README.md) in question |

Is suggested to enabled both:

### Global

The __Global__ configuration scope takes place in the main plugin configuration file, it can be found at __conf/plugins/logger/logger.conf__, it schema contains the __[LOGGER]__ section and allows the following entries:

| key | description |
|-----|-------------|
| FlushTimeout | Set the number of seconds to wait before to flush to disk. Default value is 3 seconds.|
| MasterLog | Specify the log file that will store Server events when working in daemon mode, it must be a path where the server have write access.  |

Example
```Python
[LOGGER]
    FlushTimeout 3
    MasterLog    /home/foo/monkey/logs/master.log
```

The example above sets that all Log buffers will flush to the filesystem after three seconds and for every Server event, it will be stored on the given path on __MasterLog__ key.

### Virtual Host

The __Virtual Host__ configuration scope, allow to define where each successful/error request for the given host will be stored. The following configuration takes place inside each Virtual Host file and the schema contains the __[LOGGER]__ section allowing the following entries:

| key      | description |
|----------|-------------|
|AccessLog | Define the absolute path where HTTP requests that were successful will be registered.|
|ErrorLog  | Define the absolute path where HTTP requests that failed will be registered. |

> Note that each given path must be writable by Monkey Server, otherwise the server will start triggering errors at flushing time.

An example of the configuration inside a Virtual Host is as follows:

```Python
[LOGGER]
    AccessLog /home/foo/monkey/logs/access.log
    ErrorLog  /home/foo/monkey/logs/error.log
```

Every successful request will be registered on the path given for __access.log__ file, for the opposite case, the __error.log__ file will be used. If some key is missing, the event will not be considered by __Logger__.

## About Logger internals

In technical terms, for each Virtual Host the __Logger__ plugin creates a buffer based on a Linux Kernel Pipe for each type (access/error), the common buffer size is about 65KB and for each event the running thread dispatch the log entry to the Pipe. On the other side exists a worker thread that consume the Pipes data once it reach the FlushTimeout or a Pipe capacity is near of it 90% of usage, on that moment it flush to disk each log.

For short, it runs in a lock-free mechanism without affect performance.
