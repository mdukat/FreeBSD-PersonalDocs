# Post Install
Table of contents:
 - 1) Set terminal resolution
 - 2) Enable pretty PS1 (`user@host:/# `) on user (sh/bash only)
 - 3) Cool aliases for linux users
 - 4) Check battery status (no information in handbook)
 - 5) Enable `su` access for user
 - 6) Change shell to bash, but still use `.shrc` configuration
 - 7) Quick `.profile` rebuild (Change editor, remove fortune)
 - 8) Allow users to mount CDROM/USB
   - 8.1) Fix `operator` group permissions
 - 9) Load `ext2fs` kernel module on boot

## 1) Set terminal resolution
 - [Vesa kldload fix](https://forums.freebsd.org/threads/trouble-with-changing-console-resolution.57689/)
 - [Handbook page (outdated)](https://www.freebsd.org/doc/handbook/consoles.html)
### Quick fix
 - Check if `kern.vty=vt` exists in `/boot/loader.conf`
   - If yes, change `=vt` to `=sc`
   - If no, create line `kern.vty=sc`
 - Add line `allscreens_flags="VESA_800x600"` to `/etc/rc.conf`
 - Reboot
 - Enter `vidcontrol -i mode`. It should show resolutions
 - Modify `/etc/rc.conf` with line `allscreens_flags=` to for example `allscreens_flags="MODE_279"`
 - Reboot
 
## 2) Enable pretty PS1 (`user@host:/# `) on user (sh/bash only)
 - Open your shell rc file `.shrc`
 - Uncomment lines 37:41
   - Vim oneliner: `:37,41s/# //`
 - Relog
 
## 3) Cool aliases for linux users
```
alias ls='ls --color'
alias free='vmstat'
alias lsblk='geom disk list'
```

## 4) Check battery status (no information in handbook)
[NixCraft link](https://www.cyberciti.biz/faq/freebsd-finding-out-battery-life-state-on-laptop/)
 - `apm` - Simplest to read
 - `sysctl hw.acpi.battery` - Acpi numbers
 - `acpiconf -i 0` - Voltages, capacity, OEM info...
 
## 5) Enable `su` access for user
 - As root:
   - `pw usermod your_user -G wheel`
 - As user:
   - Relog
   
## 6) Change shell to bash, but still use `.shrc` configuration
 - As root: `pkg install bash`
 - As user:
   - `chsh -s /usr/local/bin/bash`
   - `mv ~/.shrc ~/.bash_profile`
   - relog

## 7) Quick `.profile` rebuild
 - Open `.profile` with favourite editor
 - Change `EDITOR=vi` on line 18, to whatever you want (ex. `EDITOR=vim; export EDITOR`)
 - Comment out last line (put `# ` before line)

## 8) Allow users to mount CDROM/USB
[NixCraft link](https://www.cyberciti.biz/faq/freebsd-allow-ordinary-users-mount-cd-rom-dvds-usb-removabledevice/)
 - As root:
   - `echo "vfs.usermount=1" >> /etc/sysctl.conf`
   - `sysctl vfs.usermount=1`
   - For USB drives:
   ```
   echo "# USB Devices for Operator group" >> /etc/devfs.conf
   echo "own   /dev/da0   root:operator" >> /etc/devfs.conf
   echo "perm  /dev/da00  0666" >> /etc/devfs.conf
   ```
   - For CDROM:
   ```
   echo "# allow member of operator to mount cdrom" >> /etc/devfs.conf
   echo "own	      /dev/cd0	   root:operator" >> /etc/devfs.conf
   echo "perm      /dev/cd0	   0660" >> /etc/devfs.conf
   ```
   - Add user to group `operator`: `pw groupmod operator -m your_user`
 - As user:
   - Make directory for mounting: `mkdir ~/mountpoint`
     - Mount FAT32 pendrive: `mount_msdosfs /dev/da0 ~/mountpoint`
     - Mount CD9660 CDROM: `mount_cd9660 /dev/cd0 ~/mountpoint`
   - Unmount: `umount ~/mountpoint`
   
## 8.1) Fix `operator` group permissions
[Katron blog entry](https://katron.org/blog/2018/06/freebsd-devfs-rules/)
 - As root:
   - Open `/etc/devfs.rules` with your editor
   - Add these lines:
   ```
   [localrules=10]
   add path 'da*' mode 0660 group operator
   ```
   - Open `/etc/rc.conf` with your editor
   - Add this line:
   ```
   devfs_system_ruleset="localrules"
   ```
   - Restart devfs: `/etc/rc.d/devfs restart`
   - Check if drive now has good permissions: `ls -al /dev/da*`
   - Should be: `crwx-rw---- 1 root operator 0x6b Nov 2 17:42 /dev/da0s1`

## 9) Load `ext2fs` kernel module on boot
[/boot/loader.conf man page](https://www.freebsd.org/cgi/man.cgi?query=loader.conf&sektion=5&apropos=0&manpath=FreeBSD+12.0-RELEASE+and+Ports)
 - As root:
   - Open `/boot/loader.conf` with your editor
   - Add line `ext2fs_load="YES"`
   - Reboot
   - Check with `kldstat`
