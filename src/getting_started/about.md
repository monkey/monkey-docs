# About Monkey

Monkey HTTP Server, is a very Fast and Lightweight Web Server for Linux platforms. What make it so fast ?, It's written in C and use an hybrid networking model based on fixed threads,polling and asynchrounous calls, providing a real scalable web server with low CPU and memory consumption. Performance is our main goal.


In addition, Monkey HTTP Server tries to take the most of Linux Kernel. As opposite to many other similar projects, Monkey is not portable between different operative systems, it tries to use specific Linux syscalls available to gain great performance. Also, Monkey can be easily extended, it provides a flexible API to create plugins which are loaded on startup per configuration.
