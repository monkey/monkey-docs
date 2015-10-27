# Raspberry Pi

[Monkey](http://monkey-project.com) is fully supported on the [Raspberry Pi](http://raspberrypi.org) board. If you want to use Monkey in your Raspberry Pi device, you can build it from sources as described in the [Getting Started](../getting_started/README.md) section or use our binary __Debian__ packages that can be obtained from our special APT repository for Raspbian.

Note that starting from Monkey v1.6.4, we only distribute packages for [Raspbian Jessie](https://www.raspberrypi.org/blog/raspbian-jessie-is-here/).

## Server GPG key

The first step is to add our server GPG key to your keyring, on that way you can get our signed packages:

```shell
$ wget -qO - http://apt.monkey-project.com/monkey.key | sudo apt-key add -
```

## Update your sources lists

On Debian and derivated systems such as Raspbian, you need to add our APT server entry to your sources lists, please add the following content at bottom of your /etc/apt/sources.list file:

```shell
deb http://apt.monkey-project.com/raspbian jessie main
```

then update your repository cache with:

```shell
$ sudo apt-get update
```

## Install Monkey

Using the apt-get command you are able now to install the latest version of Monkey HTTP Server:

```shell
$ sudo apt-get install monkey
```
