# FastCGI

FastCGI Plugin is the client side implementation of the FastCGI protocol for interfacing interactive programs with a HTTP Server like Monkey.

In the old fashion mechanism called _CGI_, to trigger the execution of a script or a binary program, the Server requires to spawn a new process context, this new process is in charge to execute the target file or run it through an interpreter, this is a very expensive procedure at Operating System level. As opposite, __FastCGI__ implements a client/server model where the FastCGI server side, make sure the target resource program is running in an infinite loop and will only by called upon request, when done instead of exit, the program is started again inside the same process context, avoiding process creation waste. This is usually provided by a third party program and for different Script languages exists many FastCGI server implementations.

Using FastCGI is highly recommended when running scripting web programs such as Python, PHP, Perl or any other.

## Enable Plugin

To enable the __FastCGI__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-fastcgi.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

The _FastCGI_ plugin takes a global setup that affect all Virtual Hosts at once on the configuration file __conf/plugins/fastcgi/fastcgi.conf__. The configuration schemas defines two available sections: __[FASTCGI_SERVER]__ and __[FASTCGI_LOCATION]__:

### FASTCGI_SERVER

The section named __FASTCGI_SERVER__ aims to represent a FastCGI backend server that is listening for incoming FastCGI requests. It supports four main keys: ServerName, ServerAddr, ServerPath and MaxConnections.

| key | description |
|-----|-------------|
|ServerName | Local name to represent the FastCGI server. |
|ServerAddr | If the FastCGI Server is listening on a TCP connection, use this key to specify as value the Host IP and target TCP port, e.g: 192.168.0.4:9000. |
|ServerPath | Use this key if the FastCGI Server is listening in a local Unix socket file.|
|MaxConnections | Set the maximum number of open connections to the FastCGI Server. This number is a total across Monkey workers.|

> Note: You may use ServerAddr or ServerPath, not both.

### FASTCGI_LOCATION

This section aims to trap a HTTP request that targets a specific URL location. So when a match occurs, the FastCGI plugin redirect the requests to the _FASTCGI_SERVER_ associated on this Location definition. The section supports four main keys: LocationName, ServerNames, KeepAlive and Match:

| key | description |
|-----|-------------|
| LocationName | Local name to represent this location |
| ServerNames  | Space separated list of FastCGI Servers (required) |
| KeepAlive    | Keep FastCGI server connections alive. Make sure to tune the MaxConnections on __[FASTCGI_SERVER]__ directives and the Worker count on __[SERVER]__ in __conf/monkey.conf__ to avoid threads stall if available concurrent connections is less than the number of workers. Boolean value, it accepts __on__ or __off__ (default: __off__)|
| Match        | Space separated list of regular expression.|

### Configuration Example for PHP-FPM

The following example aims to provide a setup to make Monkey FastCGI plugin talk to a FastCGI Server that process PHP scripts: __php5-fpm__.

```Python
[FASTCGI_SERVER]
	ServerName     php5-fpm1
	ServerPath     /var/run/php5-fpm.sock
	MaxConnections 20

[FASTCGI_LOCATION]
	LocationName   php5_location
	ServerNames    php5-fpm1
	KeepAlive      On
	Match          /*.php
```

This example will pass the control of each request to a file ending in __.php__ to the FastCGI server running on the Unix socket __/var/run/php5-fpm.sock__ file.
