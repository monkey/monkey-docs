# Raspberry Pi

[Monkey](http://monkey-project.com) is fully supported on the [Raspberry Pi](http://raspberrypi.org) board. If you want to use Monkey in your Raspberry Pi device, you can build it from sources as described in the [Getting Started](../getting_started/README.md) section or use our binary __Debian__ packages that can be obtained from our special APT repository for Raspbian.

## Installing using APT

In your __/etc/apt/sources.list__ configuration file append the following at the end:

```Shell
deb http://packages.monkey-project.com/primates_pi primates_pi main
```

Then update your cache references:

```Shell
$ sudo apt-get update
```

Now you will be able to install Monkey on your board:

```Shell
$ sudo apt-get install monkey
```

After that last step, Monkey will be already running , you can do a simple test with __curl__ to test connectivity:

```Shell
curl -i http://raspberry_ip:2001/
```

## Using Secure Socket Layers (SSL)

Monkey supports SSL through the [PolarSSL](http://polarssl.org) library. In order to install the SSL plugin and it dependencies run the following command:

```Shell
$ sudo apt-get install monkey-polarssl libpolarssl
```

the next steps are configure Monkey to use the monkey-polarssl plugin as the default transport layer. For more details please refer to the [PolarSSL Plugin](../plugins/polarssl.md) section.
