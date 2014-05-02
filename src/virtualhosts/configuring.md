# Configuring Virtual Hosts

In [Monkey](http://monkey-project.com), every resource it's mapped under a Virtual Host. Every Virtual Host is defined by creating a configuration file under the __conf/sites/__ directory. Thus, a minimal required configuration is to having one virtual host, for that end is required the existence of at least one virtual host definition-file called __default__.

A Virtual Host definition, contains at least two sections named __HOST__ and __ERROR_PAGES__:

##  HOST

This section contains relevate core configuration for the Virtual Host it self such as it's name and where the files are located.

```
[HOST]
    ServerName   127.0.0.1
    DocumentRoot /home/foo/monkey/htdocs
```

### ServerName

It represents the Virtual Host name, it can be an IP address or any valid domain. The value of the ServerName key allows to set multiple names, e.g:

```
ServerName monkey-project.com foo.monkey-project.com bar.monkey-project.com
```

### DocumentRoot

Each Virtual Host needs to know from where it should serve it's content. The DocumentRoot key represents the absolute path for a directory that contains static content to be exposed tthrough the Server, e.g:

```
DocumentRoot /home/foo/monkey/htdocs
```

> The path must be a valid directory.


## ERROR_PAGES

When the resource requested by the HTTP client do not exists or cannot be accessed by some reason, a generic built-in page is serve explaining the nature of the problem. The __ERROR_PAGES__ section defined in the Virtual Host configuration, allow to specify custom HTML pages to be serve when a specific HTTP error status is faced, an example of common errors are:

| Code | Description |
| ----|------------ |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error|

To add custom HTML pages and associate them to a specific error status code, the Key name is the status code and the value the static file that exists under the Virtual Host in context, e.g:

```
[ERROR_PAGES]
    404  404.html
```

On this example we are defining that for all HTTP 404 status code, the Server will send back as content body the file 404.html located under the DocumentRoot of this Virtual Host.

## Plugins

Every Plugin may be able to define it owns sections on each Virtual Host, as an example we can mention the __Logger__ plugin which defines rules for files where it will write server events. For every specific section found different than __HOST__ and __ERROR_PAGES__, please refer to the plugin documentation for more details.
