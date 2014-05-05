# Mandril Security Plugin

__Mandril is a plugin which provides a security layer to Monkey through rules which can be applied to the request URI or by network address.

## Introduction

__Mandril__ is a core plugin which allow to define security rules to be applied to the incomming connections. If the client is rejected by some of these rules, it will get the 403 Forbidden error status.

## Enable Plugin

To enable the __Mandril__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-mandril.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

It currently supports two restriction modes, by request URI or by IP (fixed IP or network range), all these rules take place in the __mandril.conf__ configuration file, you can find it under __conf/plugins/mandril/mandril.conf__.

### Restriction by request URI

An URI represent the relative address of the resource requested through HTTP, a common request looks like this:

```
GET /documents/ HTTP/1.1
Host: example.com
```

If we split the first request line in three parts, the first one is the HTTP method, the second the URI and the third one the protocol version used.

So if you look to restrict the access to some client requesting a specific URI or using a specific keyword on it, you can add that rule to the __RULES__ section, e.g:

```
[RULES]
    URL documents
    URL pictures
    URL /private
```

### Restriction by IP or Network range

You can define multiple rules to deny the access to a specific IP address or a network range, most well known as a sub-network, e.g:

```
[RULES]
    IP  10.20.1.1/24
    IP 192.168.3.150
```

In the first rule we are blocking a range of IPs from 10.20.1.1 to 10.20.1.255. In the second example just one specific IP address. You can add as many restriction by IP as you want.

### Mixing rules

You can mix different rules type under the __RULES__ section, check the following example which uses the data specified previoulsy:

```
[RULES]
    URL documents
    URL pictures
    URL /private
    IP  10.20.1.1/24
    IP  192.168.3.150
```

Make sure to restart Monkey every time you edit the configuration file, so the changes can take effect.
