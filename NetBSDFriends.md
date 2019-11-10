# Quick look around on NetBSD

## Using pkg_add
```
PATH="/usr/pkg/sbin:$PATH"
PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/OPSYS/ARCH/VERSIONS/All/"
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
