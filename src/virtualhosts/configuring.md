# Configuring Virtual Hosts

In [Monkey](http://monkey-project.com), every resource it's mapped under a Virtual Host. Every Virtual Host is defined by creating a configuration file under the __conf/sites/__ directory. Thus, a minimal required configuration is to having one virtual host, for that end is required the existence of at least one virtual host definition-file called __default__.

A Virtual Host definition, contains the following section definitions:

| Section name | Description                                       |
|--------------|---------------------------------------------------|
| HOST         | Main definitions of the virtual host in question  |
| ERROR_PAGES  | Enable map of HTTP error codes with static pages  |
| HANDLERS     | Specify URL handlers through regular expressions (e.g: CGI, FastCGI) |


##  HOST

This section contains relevate core configuration for the Virtual Host it self such as it's name and where the files are located. The keys available are:

| Key             | Description                                 |
|-----------------|---------------------------------------------|
| ServerName      | Specify domain name                         |
| DocumentRoot    | Absolute path for virtual host content      |
| Redirect        | Redirect requests to a different _Location_ |

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


### Redirect

In some cases you may want the HTTP server redirects each incoming request to the Virtual Host in question to a different location. For that purpose the key __Redirect__ exists and can be assigned to any valid HTTP URL value, e.g:

```
Redirect http://monkey-project.com
```

Just note that the HTTP request will not be processed and just a HTTP redirect response will be send to the client.

## ERROR_PAGES

When the resource requested by the HTTP client do not exists or cannot be accessed by some reason, a generic built-in page is serve explaining the nature of the problem. The __ERROR_PAGES__ section defined in the Virtual Host configuration, allow to specify custom HTML pages to be serve when a specific HTTP error status is faced, an example of common errors are:

| Code | Description           |
| -----|-----------------------|
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |

To add custom HTML pages and associate them to a specific error status code, the Key name is the status code and the value the static file that exists under the Virtual Host in context, e.g:

```
[ERROR_PAGES]
    404  404.html
```

On this example we are defining that for all HTTP 404 status code, the Server will send back as content body the file 404.html located under the DocumentRoot of this Virtual Host.

## Handlers

A _Handler_ section defines the association of specific HTTP requests URI and plugin handlers through regular expression rules. A Virtual Host can define multiple _handlers_ and these are processed in order from top to bottom, the one that matches will own the responsibility to perform some action.

_Handlers_ are implemented as plugins (static or dynamic), so the handler associated  needs to be enabled at runtime and general configuration format is the following:

```
Match  REGULAR_EXPRESSION  PLUGIN_SHORTNAME
```

The following example demonstrate how a Virtual Host defines rules for HTTP requests that it's URI ends in __.php__ and other to process CGI scripts:

```python
[HANDLERS]
    Match  /.*\.php          fastcgi
    Match  /cgi-bin/.*\.cgi  cgi
```

## Plugins

On the Virtual Host configuration file, each plugin may define it owns sections, as an example we can mention the __Logger__ plugin which defines rules for files where it will write server events. For every specific section found different than __HOST__, __ERROR_PAGES__ and __HANDLERS__, please refer to the plugin documentation for more details.
