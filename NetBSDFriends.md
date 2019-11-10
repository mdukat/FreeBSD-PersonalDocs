# Quick look around on NetBSD

## Using pkg_add
[Using pkgsrc](https://www.netbsd.org/docs/pkgsrc/using.html)
```
PATH="/usr/pkg/sbin:$PATH"
# PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/OPSYS/ARCH/VERSIONS/All/"
PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/8.1/All/"
export PATH PKG_PATH

pkg_add vim
pkg_add gcc6
```

## DHCP Client
 - `dhclient`
 - [Setting up TCP/IP](https://www.netbsd.org/docs/guide/en/chap-net-practice.html#chap-net-practice-small-net)
 
## Manual network setting + routing
```
ifconfig
ifconfig wn0 inet 10.0.2.5
route show
```

## Running gcc (not in $PATH)
```
find / -name gcc
```
