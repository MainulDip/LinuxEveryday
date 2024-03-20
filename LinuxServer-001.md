### Overviews:

* BIOS : Basic Input Output System. Its the very first sector of a hard drive, where boot information is stored (called boot sector)
* UEFI : Unified Extensible Firmware Interface. Its a dedicated partition on hard drive.

* GRUB & GRUB2 : Boot loaders. GRUB2 is the latest iteration and uses `grub.cfg` as configuration file. GRUB Legacy uses `menu.list` and `grub.conf` as configuration file.

GRUB2 is customizable in `/ctc/default/grub/`, hidden boot menu (press shift to get boot log) , can boot ISO, USB, UUID, device

### Linux Kernel Loading:

### Managing Services with `systemctl` (systemD):
SysVInit cannot start/stop 2 services at the same time. `systemD` is the modern approach to handle such cases. And the `systemctl` commands are the way to  interact with systemd.

`systemctl` - list all unites
`systemctl list-units --type=service --state=active` or `systemctl --type=service --state=active` - list all unites those are services

`systemctl --type=service | grep docker` - list docker services
`systemctl enabled docker` - will start docker automatically when system starts/re-starts
`systemctl status/start/restart docker` - to get status, make active or restart docker service.

`systemctl enable package-name` Automatic start package on system start/restart
`systemctl disable package-name` will stop a package starting on system start/restart

* `enabled` in systemD status means, the package will start on system reboot

### `/etc/init.d` director, `service` and `chkconfig` commands (SysVInit):
`ls /etc/init.d` - list all the services/programmes installed on the computer.

all packages listed inside that can be managed by `system` command for immediate actions. But for set auto start behavior on system start/re-start use `chkconfig` command.

`system start docker` - will start docker. There are `status/start/restart` commands

`chkconfig --list package-name` - will print run levels of the package. 

* Run Levels:

0: System halt i.e., the system can be safely powered off with no activity.
1: Single user mode
2: Multiple user mode with no NFS (network file system)
3: Multiple user modes under the command line interface (not gui)
4: User-definable.
5: Multiple user mode under GUI (standard runlevel )
6: Reboot which is used to restart the system

* Using `chkconfig` to set automatic package start in different run level

`chkconfig package-name on` - will set auto start value to a package on different run levels (2,3,4,5 on)

`chkconfig --level 3 package-name on` - will set auto start a run level 3
`chkconfig --list package-name` - list status of the package name

 ### nano, vi and vim text editors:
`vi` - has 2 modes, insert and command mode. `a` or `i` + enter to hop as insert mode and `esc` to switch command mode. `:wa` or `:w` to save and exit the file.

`nano` is straight forward

### System wide Corn Jobs and `/etc/corn.d`:
Corn fields: there are 6 fields, minute (0-59), hour (0-23), day-of-month (1-31), month (1-12), day-of-the-week (0-6)

`*` in corn field means all
`1-7` in corn fields between every 1 and 7
`*/4` will divide that field by 4 and execute every that time. Like for minute, it will execute every 15 minute 


`ls /etc/cord.d` and will list all the corn files
`cat corn-file` from corn.d directory will show what listed there as corn task

`ls | grep corn` form /etc directory will list all corn tasks directory by fields, like `corn.daily`, `corn.hourly`, `corn.monthly` etc. Each directory contains a `logrotate` file which is a bash script that are gonna run by the time mentioned by its directory name.

* `corntab` is the special bash script for corn jobs and hold all the user modified runnable task. `corn.d` directory can also hold any custom bash script for corn jobs 

### Personalized (non root) corntab `corntab -e` and onetime job `at`:
`corntab -e` cmd will prompt a personalized corntab that will run on behalf of a user (not root)

* `at` for one time corn job. `atq` command will print the one time corn jobs queue

`at now+1 minute` cmd will prompt for what to run. like `echo "Print this" >> /home/onetimecorn.txt` will append the text after 1 minute

`at tomorrow` will for tomorrow.

### `/proc`, `/sys` and `/dev` directory:
proc stores all the processes.
sys for system info
dev for device related info