# Liana

On Monkey architecture exists the concept of __Transport Layer__, which refers to the plugin who take cares of the low level and generic Network I/O. For hence, Monkey core and the configuration needs to know which __Transport Layer__ plugin will be used when running.

Liana is the __Transport Layer__ plugin that take cares to provide the most basic functions for Network I/O over plain sockets (no encryption). The plugin do not need any configuration and is considered the most simple plugin.

In technical terms, the __Liana__ functionalities are:

* Accept an incoming connection
* Read from connection
* Write to connection
* Write Vector arrays to connection
* Close connection
* Create connection
* Connect to host
* Bind an address
* Start a listener server
* Send a file

Any other plugin that act as a __Transport Layer__ may implement the same functionalities described above, as an example of a plugin using SSL encryption refer to the [PolarSSL](polarssl.md) plugin.

## Enable Plugin

To enable the __Liana__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-liana.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.
