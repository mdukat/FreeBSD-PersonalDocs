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

## pkgsrc Python3.7 "/usr/X11R6 not found" fix
[Mailing list fix thread for Python2.7](http://mail-index.netbsd.org/pkgsrc-bugs/2017/10/24/msg062862.html)
```
make PKG_OPTIONS.python37='-x11' depends
make
make install
```

## Network
### By hand
```
ifconfig re0 inet 192.168.0.100 netmask 255.255.255.0
route add default 192.168.0.1
```
### Automatically
```
/etc/rc.conf:
ifconfig_re0="inet 192.168.0.100 netmask 255.255.255.0"
defaultroute=192.168.0.1
```

## Quick and dirty ProFTPd

> This method is dirty af, since it's working on examples, and not right rc.d file

```bash
pkg_add proftpd

vim /usr/pkg/etc/proftpd.conf
# Add DefaultAdress, remove anonymous login, modify DefaultRoot
> DefaultAddress 192.168.0.100
> DefaultRoot /hdd

crontab -e
# Add onestart on reboot, change /hdd permissions
> @reboot /usr/pkg/share/examples/rc.d/proftpd onestart
> @reboot chmod 777 /hdd

# Reboot or enable proftpd by hand
/usr/pkg/share/examples/rc.d/proftpd onestart
```

## Quick bash installation and setup
```
pkg_add bash
usermod -s /usr/pkg/bin/bash d3s
```

## Delete package with dependencies
`-R` option removes dependencies for package, unless other package uses those dependencies.
```
pkg_delete -R ffmpeg3
```

## The thing with drive letters
> The letters (a-e) are the partition names.  And partitions c and d are
> special.  Partition d stands for your whole disk, and c stands for the
> NetBSD slice.

[Re: [Ext2 Mount] Newbie Question](https://mail-index.netbsd.org/netbsd-help/2001/04/21/0002.html)
