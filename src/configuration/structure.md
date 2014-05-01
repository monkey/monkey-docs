# Configuration Structure

Monkey is configured using the old Unix way: plain text files. The configuration files are located in the __conf/__ directory and have the following structure.


    conf/
        /monkey.conf
        /monkey.mime
        /plugins.load
        /sites/
              /default
        /plugins/
                /security
                /security/security.conf

## Configuration Files and Directories

### monkey.conf

This is the principal configuration file, it describes the server setup and different variables that alter the server behavior such as number of workers, TCP port, timeouts, etc.

### monkey.mime

Each file in an operating system have a mime type associated, by convention using the file extension. It defines the data type contained, in this case, for a web server, monkey.mime contains a list of file extensions associated with the right mime type, this information is used in the HTTP protocol to communicate to the HTTP client what type of data is being transfered. If the requested file has not an extension or mime type associated, the default mime type __text/plain__ is sent.

### plugins.load

When Monkey starts up, it reads the plugins.load file and check which plugins must loaded.Each plugin is a shared library and it must be enabled on this file.

### sites/

The sites directory, specifies the virtual hosts available, each file under this directory is considerated as a virtual host definition.

### sites/default

As was described above, the sites directory contains virtual host definitions on each existent file, the __default__ file is mandatory and it defines the default virtual host configuration. In Monkey it's mandatory that __default__ file exists.

### plugins/

The plugins directory, contains specific configuration files for the running plugins.
