# SSL & TLS / HTTPS

[Monkey](http://monkey-project.com) supports Secure Socket Layers (SSL) through the [mbedTLS Library](http://polarssl.org) library wrapped up by our __mbedTLS plugin__.

## Requirements

As said, the __mbedTLS__ plugin requires to have access to the [mbedTLS Library](http://polarssl.org), we strongly suggest to use version >= 1.3.x. If that version is not available on your Linux distribution you may consider to compile it from sources. Besides that, the only extra requirement is an updated __GCC compiler__ and the __automake__ tool.

If a new [mbedTLS Library](http://polarssl.org) will be build, it will be required to have the __cmake__ tool installed on the system too.

## Build

If [mbedTLS Library](http://polarssl.org) is installed and known by the system we only need to tell Monkey to build this plugin:

```shell
$ git clone https://github.com/monkey/monkey.git
$ cd monkey
$ git checkout v1.5-fixes
$ ./configure --enable-plugins=mbedtls
$ make
```

In case you want to build your own [mbedTLS Library](http://polarssl.org) and keep it on a different path, please refer to the following steps:

```shell
$ wget https://polarssl.org/download/latest-stable
$ tar xvzf mbedtls-1.*
```

By default only the static library version is built, but we need the shared library. To start configuring and building do:

```
$ cd mbedtls-1.*
$ cmake -DUSE_SHARED_MBEDTLS_LIBRARY=on .
$ make
$ make install
$ cd -
```

After the library has been built we need to reconfigure Monkey. To help Monkey find the new [mbedTLS Library](http://polarssl.org) we provide the __include__ and __library__ paths to the Monkey __configure__ script.

```shell
$ cd monkey-1.5.6
$ ./configure --enable-plugins=mbedtls \
  --mbedtls-library=/usr/local/lib \
  --mbedtls-headers=/usr/local/include
$ make
```

## Enable Plugin

To enable the __mbedTLS__ plugin, please follow the steps mentioned on [Plugins](../configuration/plugins.md) section. The plugin name is __monkey-mbedtls.so__, so make sure the plugin entry is __Load__ and the absolute path is correct.

## Configuring

Now the plugin is enabled, but we need to instruct Monkey to use a different plugin as the __transport layer__. On the file __conf/monkey.conf__, locate the __SERVER__ section and look at the __TransportLayer__ key, we need to replace the old transport layer called __liana__ by __mbedtls__:

```Python
[SERVER]
    TransportLayer mbedtls
```

## Configuring Certificates

You may also want to edit the SSL plugin settings to use your own certificate file. The configuration file is located at __conf/plugins/mbedtls/mbedtls.conf__. The default options are given below.

```Python
[SSL]
    CertificateFile      srv_cert.pem
    CertificateChainFile srv_cert_chain.pem
    RSAKeyFile           rsa_key.pem
    DHParameterFile      dhparam.pem
```

The minimum requirement to make HTTPS works is to set __CertificateFile__ and __RSAKeyFile__. Setting __CertificateChainFile__ is recommended to speed up the handshake process, but the DHParameterFile is only necessary if your using a version of the polarssl library older than 1.2.5.

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
