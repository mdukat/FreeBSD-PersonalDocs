# CPU Scaling and Power Management

## Links
 - [powerd HOW-TO](https://forums.freebsd.org/threads/howto-freebsd-cpu-scaling-and-power-saving.172/)
 - [powerd man page](https://www.freebsd.org/cgi/man.cgi?query=powerd&sektion=8&manpath=FreeBSD+7.4-RELEASE)
 - [stress man page](https://www.freebsd.org/cgi/man.cgi?stress(1))
 
## Check supported frequencies
```
$ sysctl dev.cpu.0.freq_levels
dev.cpu.0.freq_levels: 1600/2000 1400/1720 1200/1440 1000/1160 800/800 600/600
```

## Check present frequency
```
$ sysctl dev.cpu.0.freq
dev.cpu.0.freq: 1600
```

## Change frequency using `powerd`
```
$ powerd -m 600 -M 600
```

## Change frequency on boot
 - Edit crontab using `crontab -e` as root
 - Add line `@reboot powerd -m 600 -M 600`
 - Save, exit, reboot
 
