# Liana

Network I/O operations requires a static or dynamic plugin that provides the interfaces for that purpose, __Liana__ is a network layer plugin that provides the basic interface to handle plain sockets. This plugin is built-in by default (static) and is the default interface for each Listener. The plugin do not need any configuration and it basically provide the following functionalities are:

* Read from connection
* Write to connection
* Write Vector arrays to connection
* Close connection
* Create connection
* Connect to host
* Bind an address
* Start a listener server
* Send a file
