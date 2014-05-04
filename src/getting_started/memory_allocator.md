# Memory Allocator

A __Memory Allocator__ interface is in charge to request and release chunks of memory needed by a program. Most of programs uses the solution provided by the standard LibC but this last one face many fragmentation and performance problems when used on a multithread server as this.

Starting from [Monkey v1.4](http://monkey-project.com/Announcements/v1.4.0), we have integrated the [Jemalloc](http://www.canonware.com/jemalloc/) as the default memory allocator, this is a huge step forward in performance that is visible in multi core systems under a high load.

When building Monkey through the __configure__ script, there is one step that may take a few seconds or minutes depending of the CPU speed due to [Jemalloc](http://www.canonware.com/jemalloc/) prepare it builds too. When compiling through the __make__ command the build process will take longer as the dependency library is being built. Note that  [Jemalloc](http://www.canonware.com/jemalloc/) is linked statically on Monkey binary, that means that Monkey binary size will grow to around 2MB instead of 85KB.

Even [Jemalloc](http://www.canonware.com/jemalloc/) is our new and default memory allocator, it continue being an optional feature as there is some cases where users would prefer to use their default OS memory allocator. For those cases we offer a specific configuration mode called __--malloc-libc__, it can be enabled with:

```shell
$ ./configure --malloc-libc
```

After configure with that option, make sure to perform a clean build of Monkey again:

```shell
$ make clean
$ make
```

You will see that Monkey builds times faster than before and it's binary size is reduced to less than 90KB.
