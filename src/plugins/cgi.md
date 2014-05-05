# CGI - Common Gateway Interface

The CGI (Common Gateway Interface) allows to interact with a third party program that process a HTTP request and generate content on the fly. This is the old-fashion mechanism to run programs through a HTTP server.

When a program runs using the CGI interface, the server will spawn a new process to make the script run on new environment, this is a known major performance issue of CGI.

> Note: If your server requires to achieve high performance, is suggested to use the FastCGI interface instead of the old-fashion CGI.

## Enable Plugin

To enable the CGI plugin, make sure it's enabled according to the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-cgi.so__, make sure the plugin is __Load__ and the absolute path is correct.

## Configuring

Once the plugin is loaded, you need to define if the CGI plugin will run for the whole server as a global feature or if it will be just for a specific sets of [Virtual Hosts](../virtualhosts/README.md).

The CGI plugin configuration can be placed on two locations basically depending of the need, if it will be applied to the whole context of the server, the configuration must go into the __conf/plugins/cgi/cgi.conf__ configuration file. In the other hand if it will be applied to a specific [Virtual Hosts](../virtualhosts/README.md), it should be placed on each [Virtual Hosts configuration file](../virtualhosts/configuring.md).

### Configuration Schema

The following schema must be applied to the target configuration file, the main section name is __[CGI]__ and it support multiple entries named __matches__ that act like rules to decide whatever the incoming request should be processed by the CGI plugin or not. The schema looks as follows:

```Python
[CGI]
    Match /cgi-bin/.*\.cgi
    Match /.*\.php /usr/bin/php-cgi application/x-httpd-php
```

In the above example we can see two rules for matching CGI programs that needs to be handled. A __Match__ rule can be __simple__ or __extended__:

|rule type | format |
|----------|--------|
| simple   | Match REGEX  |
| extended | Match REGEX  [INTERPRETER] [MIMETYPE] |


The REGEX represents a Regular expression to match the target file that is being requested, make sure it also aligns to the URL. On __simple__ mode the target file is executed directly, on the other hand the __extended__ version of the format allows to specify an Interpreter program that will be used to process the target file, a common example for this cases is a file made in Perl, Python, Lua or any scripting-like language. The last argument is an optional __MIME Type__ to be used when sending back the response, if your script do not handle this, make sure to set this field with a proper value.

### Notes

If for some reason when using the __extended__ format mode the Interpreter cannot be found or executed by Monkey, it will trigger a __500 Internal Server Error__ response to the client.
