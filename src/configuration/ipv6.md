# IPv6

The internet protocol v6 (IPv6) brings exciting features such as a hierarchical addressing, routing infrastructure and security. Monkey is a IPv6 compliant HTTP server.

## Requirements

Your Linux system must have IPv6 enabled in the Kernel and in the network interfaces. You can check if you system have IPv6 enabled with the following tricky command:

```
$ [ -f /proc/net/if_inet6 ] && echo 'IPv6 system ready!' || echo 'No IPv6 support found! Compile the kernel!!'
```

## IPv6 system ready!

If your system is ready for IPv6 you will get the message __IPv6 system ready!__, if not, we suggest to review your distribution documentation about how to make it work. Those steps are out of the scope of this document.

Now you could check which interfaces have an IPv6 address ready to use:

```
$ cat /proc/net/if_inet6
fe80000000000000a288b4fffecaf0e0 03 40 20 80    wlan0
fe800000000000005e260afffe750bce 02 40 20 80     eth0
00000000000000000000000000000001 01 80 10 80       lo
```

On this example the interfaces __wlan0__, __eth0__ and __lo__ can be used by Monkey with IPv6.

## Setup

The Monkey plugin named Liana provides the transport layer, by nature it support IPv4 and IPv6 modes, so you don't need any extra dependency, everything is already in place.

Monkey by default listen in IPv4 mode but can be configured to work in IPv6 with a simple setup, open the __monkey.conf__ configuration file and locate the __Listen__ key under the __SERVER__ section:

```Python
[SERVER]
    # Listen:
    # -------
    # The Listen directive restricts to the network interface from where Monkey
    # will be listening for incoming connections. If you want to restrict to the
    # lookback interface, for IPv4 you can set '127.0.0.1' or the value '::1' in
    # case of IPv6, e.g:
    #
    # Listen 127.0.0.1
```

As described, the Listen key have as purpose to define which interface(s) Monkey must listen, the value assiged can be the address in IPv4 or IPv6 format from the network device desired. As our purpose is to listen in IPv6 mode we can instruct Monkey to listen in all interfaces through the default IPv6 address named '::' .

```Python
[SERVER]
    Listen ::
```

Now you can open your web browser and open the URL __http://[::1]:2001/__ to check that it works properly.

In IPv6 the address format is quite different than IPv4, the localhost address is represented by '::1', all interfaces by '::'. As an example, if you want to listen only through eth0 interface which have the IPv6 address fe80::a288:b4ff:feca:f0e0 you could set the Listen as follow:

```
[SERVER]
    Listen fe80::a288:b4ff:feca:f0e0
```

Finally you can test the following URL __http://[fe80::a288:b4ff:feca:f0e0]:2001__. Now you are ready to publish your web services under IPv6 powered by Monkey.
