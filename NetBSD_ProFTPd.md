# ProFTPd installation on NetBSD from start to finish
So you want to access your server files with FTP?

> All commands that start with # are running as root

## 1. Download
### 1.1 `pkgin` method
To install ProFTPd using `pkgin` method, you need to install `pkgin` itself. If you have not installed it on system setup, check this guide on official website: [pkgin.net](http://pkgin.net/).

Now, when you have `pkgin` installed and configured, simply do:
```
# pkgin install proftpd
```
[comment]: <> (TODO: add output of pkgin, and complete this subchapter)

`pkgin` should show you packages needed to download, and ask for acceptance. This is however not everything! You still need to configure ProFTPd. Jump to chapter 2. - Configuring ProFTPd.

### 1.2 `pkg_add` method
To install ProFTPd using `pkg_add` method, you simply need to export PKG_PATH. You can do that, like this:
```
# PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/8.1/All/"
# export PKG_PATH
```

> Warning! PKG_PATH is dependent from architecture, and version of NetBSD you are using. Check version using `uname -a`

Now, since you got PKG_PATH exported, you can use `pkg_add` almost like `pkgin`:
```
# pkg_add proftpd
```
[comment]: <> (TODO: add output of pkg_add, and complete this subchapter)

`pkg_add` should now install your proftpd files with dependencies. This is however not everything! You still need to configure ProFTPd. Jump to chapter 2. - Configuring ProFTPd.

### 1.3 `pkgsrc` method
[comment]: <> (TODO: finish me)
> Chapter in build!

## 2. Configuring ProFTPd
### 2.1 Copy example configuration
