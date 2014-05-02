# Basic Authentication

Monkey provides a basic authentication mechanism through the Auth plugin, this authentication method is described in the RFC2617.

In order to enable basic authentication (basic auth for short) in Monkey you must check that Auth plugin is part of your Monkey installation. If you installed Monkey from sources the plugin will be there.

The basic auth works is designed to work at Virtual Host level, so you are able to protect specific Virtual Hosts and apply different set of users for each one. Please refer to the following section about how to setup basic auth.

## Configuring

Make sure __Auth plugin__ is enabled in your Monkey configuration, open your __conf/plugins.load__ configuration file and find a similar entry like this:

```Python
[PLUGINS]
    # HTTP Basic Authentication
    # =========================
    #
    Load /home/foo/monkey/plugins/auth/monkey-auth.so
```

Once the plugin is loaded you want to have a list of users who will access the Virtual Host. The Auth plugin distribute a tool named mk_passwd which allows to manage users in a static file. This tool is pretty similar to htpasswd from Apache but the format to store the users is different.

Create an initial users file with one user:

```
$ mk_passwd -c -b /etc/monkey/plugins/auth/users.mk  myuser mypassword
```

Now you will have a users.mk file containing the new user, the file content should looks like:

```
$ cat users.mk
myuser:{SHA1}kd/Z3bQZiv/FwZTNjObTOP3kcOI=
```

You can add subsequent users with the same command but omitting the __-c__ flag as this instruct mk_passwd to create a new file, e.g:

```
$ mk_passwd -b /etc/monkey/plugins/auth/users.mk  my_second_user some_password
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

Now restart [Monkey](http://monkey-project.com) and reload your web page, you will get the authentication box for the specific virtual host. If you add/delete users you will need to restart the service to make the changes take effect.
