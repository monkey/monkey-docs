# Embedded Linux

[Monkey](http://monkey-project.com) not only target High-End production servers, it also target any device running Embedded Linux. It small architecture, resources optimization and Linux Kernel feature aware, makes Monkey the right choice as HTTP Server or Service stack to interface peripherals or any common scripting need on an embedded environment.

[Monkey](http://monkey-project.com) is fully supported on ARM processors and have been tested successfully on [Android](http://developer.android.com/index.html) based devices, [Gumstix](http://gumstix.org/) boards, [Raspbian](http://www.raspbian.org/) ([Raspberry Pi](http://www.raspberrypi.org/)) and [Yocto Project](https://www.yoctoproject.org/) based systems.

## Notes

When building [Monkey](http://monkey-project.com) for an Embedded system, you may be interested into the size used by the binary program. As stated in the [Memory Allocator](../getting_started/memory_allocator.md) section, [Monkey](http://monkey-project.com) is built with Jemalloc statically by default so the binary size grows. Here is a table about estimated sizes for the binary program depending of it setup:

| memory allocator | not stripped | stripped    |
|----------------- |--------------| ----------- |
| Jemalloc         |   2MB        | 331KB       |
| System default   | 111KB        |  89KB       |

> Jemalloc is highly suggested as it reduce memory fragmentation and scale on demand, for hence it helps to improve performance.
