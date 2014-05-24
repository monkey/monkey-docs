# Plugins

[Monkey](http://monkey-project.com) on it's architecture, have been designed to be a small core that serves HTTP/1.1 and also provides an API to extend the core behavior.

Plugins are shared library loaded on startup once the Server reads the content of the __plugins.load__ configuration file. The current plugins distributed on Monkey sources allows to provide the following extra features:

 * [Basic Authentication](basic_auth.md)
 * [Cheetah! Shell](cheetah_shell.md)
 * [CGI](cgi.md)
 * [Directory Listing](dirlisting.md)
 * [FastCGI](fastcgi.md)
 * [Log Writer](log_writer.md)
 * [Liana: plain sockets transport](liana.md)
 * [Mandril Security](mandril_security.md)
 * [Secure Socket Layer (SSL/HTTPS)](polarssl.md)
