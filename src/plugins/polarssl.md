# Secure Socket Layers - SSL/HTTPS

[Monkey](http://monkey-project.com) supports Secure Socket Layers (SSL) through the [PolarSSL Library](http://polarssl.org) library wrapped up by our __PolarSSL plugin__.

## Requirements

As said, the __PolarSSL__ plugin requires to have access to the [PolarSSL Library](http://polarssl.org), we suggest to use any version >= 1.3. Besides that the only extra requirement is an updated __GCC compiler__ and the __automake__ tool.

If a new [PolarSSL Library](http://polarssl.org) will be build, it will be required to have the __cmake__ tool installed on the system too.

## Build

If [PolarSSL Library](http://polarssl.org) is installed and known by the system we only need to tell Monkey to build this plugin:

```shell
$ git clone https://github.com/monkey/monkey.git
$ cd monkey
$ ./configure --enable-plugins=polarssl
$ make
```

In case you want to build your own [PolarSSL Library](http://polarssl.org) and keep it on a different path, please refer to the following steps:

```shell
$ wget https://polarssl.org/download/latest-stable
$ tar xvzf polarssl-1.*
```

By default only the static library version is built, but we need the shared library. To start configuring and building do:

```
$ cd polarssl-1.*
$ cmake -DUSE_SHARED_POLARSSL_LIBRARY=on .
$ make
$ cd -
```

After the library has been built we need to reconfigure Monkey. To help Monkey find the new [PolarSSL Library](http://polarssl.org) we provide the __include__ and __library__ paths to the Monkey __configure__ script. The library path must be added both to the runtime search path __-Wl,-rpath__ and the linker's search path __-L__. This is required as plugins and all their dependencies are loaded at runtime.

```shell
$ cd monkey
$ CFLAGS="-Ipath/to/polarsssl/include" \
> LDFLAGS="-Lpath/to/]olarssl/library -Wl,-rpath=path/to/polarssl/library" \
> ./configure --enable-plugins=polarssl
$ make
```

## Configuring Monkey for HTTPS

To let Monkey use HTTPS, we need to load the __PolarSSL Plugin__ and use it as the default __transport layer__. In the file __conf/plugins.load__ make sure the plugin is enabled:

```Python
[PLUGINS]
    Load path/to/monkey-polarssl.so
```

> The line needs to be uncommented, also verify the path points to the right __monkey-polarssl.so__.

Now the plugin is enabled, but we need to instruct Monkey to use a different plugin as the __transport layer__. On the file __conf/monkey.conf__, locate the __SERVER__ section and look at the __TransportLayer__ key, we need to replace the old transport layer called __liana__ by __polarssl__:

```Python
[SERVER]
    TransportLayer polarssl
```

## Configuring Certificates

You may also want to edit the SSL plugin settings to use your own certificate file. The configuration file is located at __conf/plugins/polarssl/polarssl.conf__. The default options are given below.

```Python
[SSL]
    CertificateFile      srv_cert.pem
    CertificateChainFile srv_cert_chain.pem
    RSAKeyFile           rsa_key.pem
    DHParameterFile      dhparam.pem
```

The minimum requirement to make HTTPS works is to set __CertificateFile__ and __RSAKeyFile__, but for development purposes no changed are needed as built-in cert files may be used instead.

Setting __CertificateChainFile__ is recommended to speed up the handshake process, but the DHParameterFile is only necessary if your using a version of the polarssl library older than 1.2.5.

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
