# SSL/TLS (HTTPS)

[Monkey](http://monkey-project.com) supports Secure Socket Layers (SSL) and TLS through the [mbedTLS library](http://tls.mbed.org), our __TLS__ plugin provides Network I/O on top of that library.

## Requirements

Starting from _Monkey v1.6_, we distribute the mbedTLS library as part of our sources, but that dependency is just build if you enable the __TLS__ plugin. The library will be linked statically.

If for some reason you want to use a different [mbedTLS library](http://tls.mbed.org) , you can use the optional __--mbedtls-shared__ configure script option for that purpose or it equivalent __-DWITH_MBEDTLS_SHARED=1__ CMake option.

## Build

Enable TLS support in Monkey is a straightforward step, you only need to tell Monkey to include the __TLS__ plugin in the build phase:


```shell
$ ./configure --enable-plugins=tls
$ make
```

In case you want to build your own [mbedTLS](http://tls.mbed.org) library and keep it on a different path, please refer to the following steps:

```shell
$ wget wget https://tls.mbed.org/download/latest-stable
$ tar xvzf mbedtls-2.*
```

By default only the static library version is built, but we need the shared library. To start configuring and building do:

```
$ cd mbedtls-2.*
$ cmake -DUSE_SHARED_MBEDTLS_LIBRARY=on .
$ make
$ make install
```

then let Monkey use that shared library version:

```shell
$ ./configure --enable-plugins=tls --mbedtls-shared
$ make
```

## Enable Plugin

If the plugin have not been built in static mode (check with _'$ monkey -b'_), you can enable the __TLS__ plugin through the follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-tls.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

As specified on the [Server](../configuration/server.md) configuration section, the Listeners are the ones who use this plugin interface. To enable SSL/TLS on a listener just append the __ssl__ keyword at the end of the Listener definition. This is an example from _conf/monkey.conf_:

```Python
[SERVER]
    Listen 443 ssl
```

With that setup, we have instructed that the Listener on TCP port 443 will use our TLS plugin that provides _ssl_ capabilities.

## Configuring Certificates

You may also want to edit the TLS plugin settings to use your own certificate files. The configuration file is located at __conf/plugins/tls/tls.conf__. The default options are given below.

```Python
[SSL]
    CertificateFile      srv_cert.pem
    CertificateChainFile srv_cert_chain.pem
    RSAKeyFile           rsa_key.pem
    DHParameterFile      dhparam.pem
```

The minimum requirement to make HTTPS works is to set __CertificateFile__ and __RSAKeyFile__, but for development purposes no changed are needed as built-in cert files may be used instead. Setting __CertificateChainFile__ is recommended to speed up the handshake process.

### Generate your own certificates

The mandatory certificate and RSA key can be generated with the following command using the OpenSSL tool:

```shell
$ openssl genrsa -out rsa_key.pem 1024
$ openssl req -new -x509 -key rsa_key.pem -out srv_cert.pem -days 1095
```

To generate a file with Diffie-Hellman parameters you can run:

```shell
$ openssl dhparam -out dhparam.pem 2048
```

## Running the server

[Monkey](http://monkey-project.com) is now able to serve content over SSL. Add your files to the DocumentRoot and start the Monkey!.

```shell
$ bin/monkey
```
