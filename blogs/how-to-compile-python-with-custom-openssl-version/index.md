<!-- 
.. title: How to compile python with custom OpenSSL version
.. slug: how-to-compile-python-with-custom-openssl-version
.. date: 2017-02-15 13:34:51 UTC+07:00
.. tags: 
.. category: blogs
.. link: 
.. description: 
.. type: text
-->

### Introduction

There's a chance your `OpenSSL` or `Python` installation is so old but it can not be updated via casual way by using **apt-get** / **yum**

In that case, *compiling from source* is the only option to upgrade these to latest version.

This tutorial will guide you through steps need to do to complete the above task.

<!-- TEASER_END: Read more -->
### Compile OpenSSL

```
cd /tmp
curl https://www.openssl.org/source/openssl-1.0.2k.tar.gz | tar zxvf -
cd openssl-1.0.2k
./config shared --prefix=/usr/local/ssl
make && make install
```

### Compile Python

```
cd /tmp
wget http://python.org/ftp/python/2.7.13/Python-2.7.13.tar.xz
tar xf Python-2.7.13.tar.xz
cd Python-2.7.13/
```

Edit the file `Modules/Setup.dist` - line `218 - 221`

```bash
# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
SSL=/usr/local/ssl
_ssl _ssl.c \
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
-L$(SSL)/lib -lssl -lcrypto

```

Continue with:

```
./configure --prefix=/usr/local/python2.7.13 --enable-unicode=ucs4 --enable-shared
```

Make symbolic links for custom OpenSSL libraries

```
ln -s /usr/local/ssl/lib/libssl.so.1.0.0 /usr/lib64/libssl.so.1.0.0
ln -s /usr/local/ssl/lib/libcrypto.so.1.0.0 /usr/lib64/libcrypto.so.1.0.0
```

Finally:

```
make && make install
```

### Testing & Create virtualenv based on custom python version

Run the following lines:

```
/usr/local/python2.7.13/bin/python -c "import ssl; print ssl.OPENSSL_VERSION"
```

Result:

```
OpenSSL 1.0.2k 26 Jan 2017
```

Yay!

Now try to create virtualenv

```
virtualenv -p /usr/local/python2.7.13/bin/python venv
```

Test one more time

```
(venv)[root@localhost /]$ python -c "import ssl; print ssl.OPENSSL_VERSION"
OpenSSL 1.0.2k 26 Jan 2017
```