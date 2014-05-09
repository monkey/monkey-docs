# About Monkey

[Monkey HTTP Server](http://monkey-project.com), is a very Fast and Lightweight Web Server for Linux platforms. What make it so fast ?, It's written in C and use an hybrid networking model based on fixed threads, pooling and asynchrounous calls, providing a real scalable web server with low CPU and memory consumption. Performance is our main goal.

In addition, [Monkey](http://monkey-project.com) tries to take the most of [Linux Kernel](http://kernel.org). As opposite to many other similar projects, Monkey is not portable between different Operating Systems, it tries to use specific Linux system calls available to gain great performance. Also, Monkey can be easily extended, it provides a flexible API to create plugins which are loaded on startup per configuration.
