# Post Install
Table of contents:
 - 1) Set terminal resolution
 - 2) Enable pretty PS1 (`user@host:/# `) on user (sh/bash only)
 - 3) Cool aliases for linux users
 - 4) Check battery status (no information in handbook)
 - 5) Enable `su` access for user
 - 6) Change shell to bash, but still use `.shrc` configuration
 - 7) Quick `.profile` rebuild (Change editor, remove fortune)

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
