# Plugins

Monkey architecture is defined by a small and flexible core that expose an API interface to extend the server capabilities and behavior through Plugins. When a plugin is enabled on Monkey, it hooks to the internals and may implement a feature that affects networking, events or the HTTP cycle.

The configuration file __conf/plugins.load__ aims to be an interface to enable or disable a Plugin. The file schema is headed by the __[PLUGINS]__ section and each entry represents a component to load, the key is always __Load__ and the value the absolute path to the Plugin, which is a shared library.

## Loading a Plugin

As an example, we will take the [Cheetah! Shell](../plugins/cheetah_shell.md) Plugin which provides a Shell interface to monitor Monkey, the configuration file should looks as follows:

```Python
[PLUGINS]
    Load  /home/foo/monkey/plugins/cheetah/monkey-cheetah.so
```

The absolute path of the Plugin may change depending of how Monkey is being used, but focusing in the example the important item is the __Load__ key and the path of the Plugin.

## Unloading a Plugin

A Plugin will not be loaded if it's not declared on the __conf/plugins.load__ configuration file or if the entry is commented. Using the above example we will comment the entry to avoid Monkey load the [Cheetah! Shell](../plugins/cheetah_shell.md) plugin:

```Python
[PLUGINS]
    # Load  /home/foo/monkey/plugins/cheetah/monkey-cheetah.so
```

## Notes

For more details about specifics of each available Plugin in Monkey, please refer to the [Plugins Chapter](../plugins/README.md) on this document.
