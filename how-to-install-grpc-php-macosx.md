# How to install Google - GRPC - Extension PHP on Mac OSX


I had to install GRPC on my Mac OSX machine and I encountered some problems while following instructions on the official documentation. Below I will explain how I managed to force the installation of it.

First, the only one installation which work is the **manual installation**. For me, the PECL installation did not work !

Official install : https://grpc.io/docs/quickstart/php.html#prerequisites

## 1 - Installation of the prerequisite

I installed GRPC on a specific folder `~/tmp`, in this tutorial we will use this folder

```
$ cd ~
$ mkdir tmp
$ cd tmp
```

Then I had some problem while installing the next steps, so I had to disable SIP on my Mac (Security Integrity Protection).
*You can do this later if you encounter some problems like me*

```
# Restart you compouter and press the CMD + R keys when it starts, you will enter in the Recovery Mode.
# Open the terminal when the Recovery mode has started
$ csrutil disable
# Restart the computer
# After that do the following command in a terminal to check if the SIP is disabled
$ csrutil status
```

To enable SIP, the command line will be in Recovery Mode `csrutil enable`.

## 2 - Installation of the GRPC C Core

Clone of GRPC from sources

```
$ cd ~/tmp
$ git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
$ cd grpc
$ git submodule update --init
$ sudo make
$ sudo make install
```

Here I encoutered a problem about a missing library `aclocal`. I did the following (be sure to have homebrew installed).

```
$ brew install autoconf automake libtool
$ brew doctor
$ brew prune
```

Redo the make install.

## 3 - Installation of the GRPC PHP Extension 

```
$ cd ~/tmp/grpc/src/php/ext/grpc
$ phpize
$ ./configure
$ sudo make
$ sudo make install
```

Here I encountered some problem about a `php.h` file missing, this file is juste a file of php dev set included in xcode command line tool. But I had to install this tool.

```
$ xcode-select --install
```

Redo the make install.

## 4 - Link GRPC php extension to my PHP 7

```
$ cd /usr/local/etc/php/7.1
$ sudo nano php.ini
# Add folling line in the php extensions (replace your username :P)
# extension=/Users/kevin/tmp/grpc/src/php/ext/grpc/modules/grpc.so
```

Enjoy ! Don't hesitate to ask me if you have other issue you resolved. We could improve this document.
