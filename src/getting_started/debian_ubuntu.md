# Debian and Ubuntu

[Monkey](http://monkey-project.com) is fully supported on Debian and Ubuntu. We provide our own APT server for packages distribution. To get started, install our GPG key and configure your repository list.

## Server GPG key

The first step is to add our server GPG key to your keyring, on that way you can get our signed packages:

```shell
$ wget -qO - http://apt.monkey-project.com/monkey.key | sudo apt-key add -
```

## Update your sources lists

On Debian and derivated systems such as Ubuntu, you need to add our APT server entry to your sources lists, please add the following content at bottom of your __/etc/apt/sources.list__ file:

#### Ubuntu 15.04 (vivid)

```
deb http://apt.monkey-project.com/ubuntu vivid main
```

#### Debian 8 (jessie)

```
deb http://apt.monkey-project.com/debian jessie main
```

### Update your repositories database

Now let your system update the _apt_ database:

```bash
$ sudo apt-get update
```

## Install Monkey

Using the apt-get command you are able now to install the latest version of Monkey HTTP Server:

```shell
$ sudo apt-get install monkey
```
