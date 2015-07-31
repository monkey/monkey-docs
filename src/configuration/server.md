# Server Core

The Server core configuration resides in the file __monkey.conf__, which is the primary and mandatory configuration file for Monkey HTTP Server. It contains many keys that can alter the server behavior, all of them are associated under the __SERVER__ section. Below a description on each one found on this Monkey version.

### Listen

On Linux as on any Unix OS, can exists many network interfaces that may represent physical or virtual Ethernet devices. The Listen key specify where the Server should listen for incoming connections, meaning the interface plus the TCP port. It must be assiged to an IPv4 or IPv6 address. By default the Server listen on all interfaces on IPv4 mode which is represented by __0.0.0.0__.

The following example will listen on all interfaces through TCP port 2001:

    Listen 2001

In the other hand, to restrict to localhost connections you can do:

    Listen 127.0.0.1 2001

The same applies for a IPv6 based interface:

    Listen ::1 2001

The Listen interface support properties. As of now there is one property available called _ssl_, which instruct the listener to use a Network plugin who handle SSL/TLS connections, e.g:

    Listen 2001 ssl

You can specify many listeners as required, a common real-world example is:

    Listen  80
    Listen 443 ssl

On this case the port __80__ is Listening for text plain connections while __443__ through some SSL/TLS layer.

### Workers

Monkey architecture is designed to work in asyncronous mode and in order to scale on SMP systems, it allows to spawn a fixed number of threads called Workers. Each worker is capable to handle thousands of connections if the system allows that.

The Workers key defines the number of Workers to spawn when Monkey starts. By default this key comes with value __0__ which aims to spawn one worker per existent CPU core.

    Workers 0

### Timeout

The Timeout key, represents the largest span of time expressed in seconds, during which a connected client should issue a complete request. The Timeout value must be > 0. By default is 15 seconds.

    Timeout 15

### PidFile

The PidFile key specifies where the Server stores the process identificator when starting up.

    PidFile /home/foo/monkey/logs/monkey.pid

### UserDir

Monkey is capable to serve public system users directories, they are accessed through the URI prefix __/~username__. The UserDir key specifies the public directory name that must be located on each user home directory.

    UserDir public_html

### IndexFile

When an incoming HTTP request arrives and it's mapped to a file system directory, the Server needs to know which file serve by default. The IndexFile key allows to define multiple files that are looked up in order, the values must be file names, not relative or absolute paths.

    Indexfile index.html index.htm index.php

### HideVersion

For security reasons users may want to hide the specific Server version, enabling this flag will turn the version to be hided when sending HTTP responses. The key accepts boolean values __on__ and __off__. By default this value is __off__.

    HideVersion off

### Resume

When serving static files, a client may want to request a range of bytes instead of a complete file. The Resume key turns this feature enabled or disabled in the Server. The key accepts boolean values __on__ and __off__. By default this value is __on__.

    Resume on

### User

The server can run as a different user at process level. To accomplish this the User key allows to define the user that the running process will change to. This will only happen if the original user have root priviledges. If this Key is commented out, no user change will be done.

    User www-data


## Advanced Configuration

The following configuration keys aims to be used by advanced users, just use them on specific cases where is really needed.

### KeepAlive

It allows to enable or disable the HTTP KeepAlive feature. The key accepts boolean values __on__ and __off__. By default this value is __on__.

    KeepAlive on

### KeepAliveTimeout

On a HTTP KeepAlive session, specify the number of seconds to wait for the next incoming request, once the KeepAliveTimeout value is reached the connection is closed. The value must be > 0. By default this value is __5__.

    KeepAliveTimeout 5

### MaxKeepAliveRequest

Under a HTTP KeepAlive session, set the maximum number of HTTP requests allowed. This value must be > 0. By default this value is __1000__.

    MaxKeepAliveRequest 1000

### MaxRequestSize

When a HTTP request arrives, Monkey allocates a fixed memory buffer. If the incoming request data is larger than the original buffer size, it will allocate more space as needed. The MaxRequestSize key defines the maximum number of KiloBytes than a buffer can grow. Example: defining __MaxRequestSize 32__ means 32 Kilobytes. The value defined must be greater than zero. Default value defined is 32.

    MaxRequestSize 32

### SymLink

A HTTP request may target a file which is in reallity a symbolic link in the file system. The SymLink key defines if the server may serve or not the linked target file. The key accepts boolean values __on__ and __off__. By default this value is __off__.

    SymLink Off

### DefaultMimeType

When a static file is requested and it does not contain a known extension based on the mime types registered on __monkey.mime__ configuration file, Monkey will send the mime type specified on this key.

    DefaultMimeType text/plain

### FDT

The File Descriptor Table (FDT) it's an internal mechanism to share open file descriptors under specific threads and virtual host context. When enabled, it helps to reduce the number of opened file descriptors for the same resource and the number of required system calls to open and close files.

The overhead in memory of this feature is around ~5KB per worker. The key accepts boolean values __on__ and __off__. By default this value is __on__.

    FDT on

### FDLimit

Defines the maximum number of file descriptors that the server can use, it can be translated to the maximum number of connections. If the value is not set, Monkey will use the soft limit imposed to the process (ulimit -n). If the variable is set, Monkey will try to increase or decrease the limit under it restrictions. For values higher that Hard Limit Monkey needs to be started by root user.

    FDT 4096
