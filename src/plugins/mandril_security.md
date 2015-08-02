# Mandril Security

__Mandril__ is a plugin which provides a security layer to Monkey through rules which can be applied to the request URI or by network address.

For every incoming connection, the plugin take the defined security rules and check at first instance the incoming IP address and then the HTTP request, if some rule matches the server will behave as follows:

* For a restricted IP address, it will drop the connection.
* For a restrictet HTTP Request, it will return a __403 Forbidden__ HTTP error.

## Enable Plugin

If the plugin have not been built in static mode (check with _'$ monkey -b'_), you can enable the the __Mandril__ plugin through the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-mandril.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

As mentioned there are two type of security mechanism that works at different stages of a live connection. Once a connection is accepted __Mandril__ will check it blacklist rules for restrictions by IP, if there is a match, it will drop the connection. The restriction by URL happens at a later stage. Both security modes requires to have a set of rules in the __conf/plugins/mandril/mandril.conf__ configuration file which are explained below.

> note: restrictions by URL requires to set a proper Handler match rule.

### Restriction by URI

An URI represent the relative address of the resource requested through HTTP, a common request looks like this:

```
GET /documents/ HTTP/1.1
Host: example.com
```

If we split the first request line in three parts, the first one is the HTTP method, the second the URI and the third one the protocol version used. So if you look to restrict the access to some client requesting a specific URI or using a specific keyword on it, you can add that rule to the __RULES__ section, e.g:

```python
[RULES]
    URL documents
    URL pictures
    URL /private
```

then is __mandatory__ to add a handler rule in your Virtual Host configuration file (e.g: __conf/sites/default__):

```python
[HANDLERS]
    Match  /.* mandril
```

without the handler __Mandril__ will not be able to apply security rules by URI once a request arrives.

### Restriction by IP or Network range

You can define multiple rules to deny the access to a specific IP address or a network range, most well known as a sub-network, e.g:

```python
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
