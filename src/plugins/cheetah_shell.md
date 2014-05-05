# Cheetah! Shell

Monkey through the plugin named Cheetah! provides a command shell which can be used to retrieve running information from the server.

Retrieve information from a running web server is very important when monitoring or troubleshooting unexpected behaviors. Cheetah! provides a command line interface to perform read-only operations (by now) about the running Monkey instance.

Cheetah! can be accessed from the command line through STDIN if Monkey did not go in background or directly through a unix pipe. Lets review how to make it work.

## Enable Plugin

To enable the __Cheetah!__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-cheetah.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

Once the plugin is loaded, we need to proceed to edit the Cheetah! configuration file which could be located in the Monkey configuration directory like __conf/plugins/cheetah/cheetah.conf__.

```Python
[CHEETAH]
    # Listen :
    # --------
    # Cheetah! listen for input commands as any shell, this can be done
    # using the standard keyboard input or through a unix pipe where you
    # need to connect using the Cheetah! client.
    #
    # The Listen directive allows you to define which input method to use.
    # Valid values for Listen are STDIN or SERVER.
    #
    Listen SERVER
```

The Listen key as described can take two values:

### STDIN

Cheetah! will listing for incoming commands from STDIN, this option requires that Monkey do not run in background mode

### SERVER

Cheetah! will open a unix socket to listen for incoming commands. If monkey runs in TCP port 2001 the unix socket filename will be located at /tmp/cheetah.2001 . Try to connect with netcat utility:

```shell
$ nc -U /tmp/cheetah.2001
*** Welcome to Cheetah!, the Monkey Shell :) ***

      << Type 'help' or '\h' for help >>

cheetah>
```

If you are connected to Cheetah! type help to see the available options:

```
$ nc -U /tmp/cheetah.2001

*** Welcome to Cheetah!, the Monkey Shell :) ***

      << Type 'help' or '\h' for help >>

cheetah> help
List of available commands for Cheetah Shell

command  shortcut  description
----------------------------------------------------
?          (\?)    Synonym for 'help'
config     (\f)    Display global configuration
plugins    (\g)    List loaded plugins and associated stages
status     (\s)    Display general web server information
uptime     (\u)    Display how long the web server has been running
vhosts     (\v)    List virtual hosts configured
workers    (\w)    Show thread workers information

clear      (\c)    Clear screen
help       (\h)    Print this help
quit       (\q)    Exit Cheetah shell :_(

cheetah>
```
