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

 ### nano, vi text editors (vim for improved vi):
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

### Network Connectivity with `ping`, `ip add` and `ip route`:
`ip add` for all ip address
`ip route` for ip routing info

### Testing DNS `ping`, `dig`, `nslookup` and `host`:
`ping` -
`dig` - `dig [@server] host.com` test and list working dns (domain name server) and A records
`nslookup` - `nslookup host.com [server]` - same like dig, name server lookup
`host` - `host host.com [server]`, show mail handler as well (`mx` record)

* localhost dns is listed inside `/etc/hosts` file

### Network config for different distro:
`ubuntu` - `/etc/hosts` is the first place for dns lookup. as this is listed inside `/etc/nsswitch.conf` file

* a gui `network manager` can be used for configuring network

### Install Source Code Softwares:
3 steep process : extract, compile and install
    1 - `tar -zxvf filename.tar.gz` to unzip tar.gp file
    2 - cd into the un-zipped directory and compile. Usually there is some `Makefile` file, run that by `make` command. If there is some `config` file run that first. After compiling, run the compiles executable by just the `./compiled-filename`. But it will not install the program
    3 - to actually install move that to `/usr/local/bin` directory, ie, `sudo mv compiled-executable-file /usr/local/bin`. Or if there are some installer file after the compilation, use `make install`.

* packages installed from tarball/tar.gz source code will not auto update. Only manual update is the way

### Install `.deb` package, apt/aptitude/apt-get and `dpkg` mess:
Always use `apt` to install a `deb` package. Also don't use dpkg, because, with dpkg, package dependencies will not be installed/pulled automatically.
`apt` - Newer, Simpler, Suggested
`aptitude` - Older, works but not suggested
`apt-get` - Oldest, works but not suggested

if `dpkg packagename.deb` does not work, revert back the mess using `sudo dpkg -r packagename`

Best way is `apt`

### Red Hat Package Manger `RPM` | YUM,DUF,RPM:
YUM is for RedHat/CentOS, stands for Yellowdog Updater Modified
DNF for Fedora, Stands for Dandified YUM
RMP is the low level tool used by both YUM and DNF

* YUM is the suggested way to install on red hat distributions by `yum install package-name`, use `yum upgrade` to do both update and upgrade


### APT Package Management System:
APT host all the repositories, from where packages can be install (apt will search the specified app from the source list and install).

inside `/etc/apt` directory, `sources.list` contains all sources apps can be searched to install and `sources.list.d` is a directory which can host another `sources.list` file that will be used by default.

To add a custom repository to the `sources.list` file is a two steep process
- add the repository, ie `deb https://repositoryname.com release-type/stable repository-type/non-free/universe/`, then save and update by `apt update`
- then add `gpg` key for signing to secure form man-in-the-middle attack. To do so, first download and add the gpg-key file from the repo by `wget -q0 https://repository.com/gpg.key | apt-key add -`. All installed keys can be checked/list in the system can be viewed by `apt-key list`. Finally update using `apt update`

### PPA (alternative to APT for custom repo adding) | Personal Package Archive for Ubuntu:
This is another way of adding a repository (instead of manual `sources.list` + gpg key way). Repository can be hooked by `add-apt-repository ppa:<repository-name>`. Which will add the new repo in the `sources.list` and download/install gpg key behind the scene.


### Configure RedHat based dist's repository with YUM:
Main repository configuration RedHat/CentOS resides in `/etc/yum.conf` file. And all repositories live inside `/etc/yum.repos.d` directory.
New repo can be hooked by `yum install repo-name` and the repo will be listed inside yum.repos.d directory.

### pacman for Arch Linux:
`packman -S packagename` to install a package

### User management in Linux:
`useradd` is a low level tool, better use `adduser` command. Use `userdel` to delete user. `usermod` to modify existing user

`usermod -g/-G/-a groupname username`, modify users group, -g for primary group, -G for secondary/supplementary group and -a for appending group.

Using `-G` alone will replace previously users assigned group with new group. So better use `-a -G`. like `usermod -a -G groupname username`

### Managing Local Groups | Primary, Secondary/Supplementary:
Each user has its onw personal primary group in linux. Usually the primary group is same as username. Also there are Secondary/Supplementary groups that a user might belong to. Enrolling on Secondary/Supplementary group allows the user to access directories that the user's primary group has no access.

`groupadd`, `groupmod` and `groupdel` are most used commands to manage groups.

`groupadd somegroup` will create a new group called as is
`groups username` will list all the user's primary and secondary/supplementary groups

### Querying User Info 
`who`, `w`, `what` and `pinky` will list all the logged in users and their info
`whoami` will list current logged in user
`id username` will list info about a specific user (id, group-id and groups)
`last` will give the history of the previously logged-in users list

### User Profile | `.bash-profile`, `.bashrc`:
There are 2 types of profile, system-wide for all users and individual. There are all shell commands to setup working environment

For all users/system-wide profile, `/etc/environment` is for path variables.

when system boots and user logged in, `/etc/profile` script runs for system-wide. When user opens a terminal, for ubuntu `/etc/bash.bashrc` or `/etc/bashrc` (redhat) will run.

Also Individual profiles are also gonna load. Which are `.bashrc`, `.profile` or `bash.profile` in `/home/user/` directory.