# Basic Authentication

Monkey provides a basic authentication mechanism through the Auth plugin, this authentication method is described in the RFC2617.

In order to enable basic authentication (basic auth for short) in Monkey you must check that Auth plugin is part of your Monkey installation. If you installed Monkey from sources the plugin will be there.

The basic auth works is designed to work at Virtual Host level, so you are able to protect specific Virtual Hosts and apply different set of users for each one. Please refer to the following section about how to setup basic auth.

## Enable Plugin

If the plugin have not been built in static mode (check with _'$ monkey -b'_), you can enable the __Auth__ plugin through the following the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-auth.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

Once the plugin is loaded you want to have a list of users who will access the Virtual Host. The Auth plugin distribute a tool named mk_passwd which allows to manage users in a static file. This tool is pretty similar to htpasswd from Apache but the format to store the users is different.

Create an initial users file with one user:

```
$ mk_passwd -c /etc/monkey/plugins/auth/users.mk  myuser mypassword
```

Now you will have a users.mk file containing the new user, the file content should looks like:

```
$ cat users.mk
myuser:{SHA1}kd/Z3bQZiv/FwZTNjObTOP3kcOI=
```

You can add subsequent users with the same command but omitting the __-c__ flag as this instruct mk_passwd to create a new file, e.g:

```
$ mk_passwd /etc/monkey/plugins/auth/users.mk  my_second_user some_password
$ cat users.mk
myuser:{SHA1}kd/Z3bQZiv/FwZTNjObTOP3kcOI=
my_second_user:{SHA1}cWX21AfcL9aFKNpjJgqRPnFiPoY=
```

Edit your virtual host configuration file and specify the Auth rules, e.g: edit __/etc/monkey/sites/default__ and append:

```Python
[AUTH]
    Location /
    Title    "Let's protect our content..."
    Users    /etc/monkey/plugins/auth/users.mk
```

The __Auth__ plugin is a handler, so we need also to register it into the __HANDLER__ section as follows:

```python
[HANDLERS]
    Match  /.*  auth
```

When [Monkey](http://monkey-project.com) process the Handlers, it will match the regular expression rules for all requests, and then the __Auth__ plugin will try to match a specific location through the __[AUTH]__ section. A match through the Handler regular expression can have many locations.

Now restart [Monkey](http://monkey-project.com) and reload your web page, you will get the authentication box for the specific virtual host. If you add/delete users you will need to restart the service to make the changes take effect.
