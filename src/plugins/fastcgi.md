# FastCGI

FastCGI Plugin is the client side implementation of the FastCGI protocol for interfacing interactive programs with a HTTP Server like Monkey.

In the old fashion mechanism called _CGI_, to trigger the execution of a script or a binary program, the Server requires to spawn a new process context, this new process is in charge to execute the target file or run it through an interpreter, this is a very expensive procedure at Operating System level. As opposite, __FastCGI__ implements a client/server model where the FastCGI server side, make sure the target resource program is running in an infinite loop and will only by called upon request, when done instead of exit, the program is started again inside the same process context, avoiding process creation waste. This is usually provided by a third party program and for different Script languages exists many FastCGI server implementations.

Using FastCGI is highly recommended when running scripting web programs such as Python, PHP, Perl or any other.

## Enable Plugin

If the plugin have not been built in static mode (check with _'$ monkey -b'_), you can enable the __FastCGI__ plugin through the following the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-fastcgi.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

The _FastCGI_ plugin takes a global setup on the configuration file __conf/plugins/fastcgi/fastcgi.conf__ that affect all Virtual Hosts. The configuration schema defines the the section __[FASTCGI_SERVER]__ and is used as a reference for all handlers.

> FastCGI plugin is a handler, for hence it requires a match rule in the Virtual Host configuration file.

### Specify a handler for FastCGI

Edit your Virtual Host configuration file (e.g: _conf/sites/default) and add a rule to your __HANDLERS__ section, e.g:

```python
[HANDLERS]
    Match  /.*\.php  fastcgi
```

The match rule defined, specify that the __FastCGI__ plugin will handle all incoming request which it URI ends in _.php_. Now proceed to configurate the global FastCGI server definition.

### FASTCGI_SERVER

The section named __FASTCGI_SERVER__ aims to represent a FastCGI backend server that is listening for incoming FastCGI requests. It supports three main keys: ServerName, ServerAddr and ServerPath.

| key       | description                                 |
|-----------|---------------------------------------------|
|ServerName | Local name to represent the FastCGI server. |
|ServerAddr | If the FastCGI Server is listening on a TCP connection, use this key to specify as value the Host IP and target TCP port, e.g: 192.168.0.4:9000. |
|ServerPath | Use this key if the FastCGI Server is listening in a local Unix socket file.|

> note: You may use ServerAddr or ServerPath, not both.

### Configuration Example for PHP-FPM

The following example aims to provide a setup to make Monkey FastCGI plugin talk to a FastCGI Server that process PHP scripts: __php5-fpm__.

```python
[FASTCGI_SERVER]
	ServerName     php5-fpm1
	ServerPath     /var/run/php5-fpm.sock
```

This example will pass the control of each request to a file ending in __.php__ to the FastCGI server running on the Unix socket __/var/run/php5-fpm.sock__ file.
