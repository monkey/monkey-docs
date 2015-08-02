# CGI - Common Gateway Interface

The CGI (Common Gateway Interface) allows to interact with a third party program that process a HTTP request and generate content on the fly. This is the old-fashion mechanism to run programs through a HTTP server.

When a program runs using the CGI interface, the server will spawn a new process to make the script run on new environment, this is a known major performance issue of CGI.

> Note: If your server requires to achieve high performance, is suggested to use the FastCGI interface instead of the old-fashion CGI.

## Enable Plugin

If the plugin have not been built in static mode (check with _'$ monkey -b'_), you can enable the __CGI__ plugin through the following the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-cgi.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

The __CGI__ plugin is a handler, for hence it needs to be enabled on the Virtual Host definition under the __HANDLERS__ sections through a Match rule. The following example will define a Handler rule to make the __CGI__ plugin process all incoming request that ends in _.cgi_:

```python
[HANDLERS]
    Match  /.*\.cgi  cgi
```

Optionally, the __CGI__ plugin support an extra parameter into the Match rule to specify an interpreter, e.g:

```python
[HANDLERS]
    Match  /.*\.cgi  cgi  /bin/bash
```

This last parameter is optional and is useful for cases for content that needs to be interpreted by Perl, Python, PHP-cli or other.
