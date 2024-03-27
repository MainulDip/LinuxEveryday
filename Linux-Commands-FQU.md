### Linux Terms:
rc - stands for `run commands`
anything.d - `.d` at the end is a `directory` notation

### `source file` command:
It is used to execute a script file in current shell environment. Which updates the shell session with updated environment thats being executed from the shell file. 

The file is typically a shell script, but it can also be any text file containing a series of commands. source command is often used to set environment variables, define functions, and execute initialization scripts.

### `alias customName="linux-command"` and `unalias previous-alias` :
It defines temporary aliases for current shell session.
ex, `alias ls="ls --color=auto"` will make ls as the mentioned command alias

To make it permanent, the alias need to be put inside `~/.bash_aliases` or `~/.bashrc` file. After that, from next login/boot the aliases will be tied with the shell. To load aliases immediate to current shell, `source ~/.bash-aliases` needs to be run or the .bashrc.

### pwd and `ls -la`:
`pwd` prints current working directory and `ls -la` for detail current directory information

### `cp` - copy | `rm` - remove | `mv` - move & `sherd` - unrecoverable file deletion:

`cp file_to_copy.txt new_file.txt` to copy

`rm [flag] <file-or-directory>` to remove/delete. `-r` flag for recursive deletion of empty directory and `-rf` for force recursive deletion of a directory that contains files and/or sub

`mv` to move (or rename) files and directories. usages are like `mv <file-or-directory-or-oldname> <destination-directory-or-newName>` 

Deleting files with `shred -u file_to_shred.txt` is almost impossible to recover. 

### `history` & `man` - helper manual, `ps` & `htop` - process viewer of the machine and `kill`:
`history`command displays an enumerated list of the commands that had been used in the past.


`nan cmd` displays the manual page of any other command (as long as it has one).

`ps` lists  currently running process and some info by the current shell session

`htop` is an interactive process viewer to view and manage machine’s resources directly from the terminal. In most cases, it isn’t installed d by default

`kill <PID/program-name>` is used to kill a process by its PID (processes ID) or name.

### `./` and `./file.sh` for running executables:
these will run executable bash script

### `chmod` - change file permission mode & `passwd`
`chmod +x script` will assign executable permission  of the script file for current user

`passwd` allows you to change the passwords of user accounts. First, it prompts you to enter your current password, then asks you for a new password and confirmation.

### `cat`, `less`, `more`, `head`, `tail` and `wc` - word count:
This commands prints the contents of a file, usages are like `<cat/less/more/head/tail> filename.txt`. 
There is a `-n` flag for specifying number of lines, like `<head/tail> -n numberInt filename.txt`

`wc filename.txt` returns the number of words is the file

### `grep`, `find`:
`grep` searches for lines that match a regular expression and print them. Often used to filter any search/list results, like `listing-commands | grep filtering-text`

For searching word count of the matching result with -c flag - `grep -c "linux" long.txt`

The find command searches for files in a directory hierarchy based on a regex expression. used like `find [flags] [path] -name [expression]`
- `find ./ -name "long.txt"` simple search to this directory 
- `find ./ -type f -name "*.py"` - search directory for file/s ending with .py

### `which`, `whoami`, `whatis`, `uname` - Unix name, `neofetch`:
The `which cmd` command outputs the full path of shell commands. like `which python`

`whoami` or `echo $USER` will print current shell user

`whatis somecmd` prints a single-line description of any command, making it a helpful reference

`uname -a`, short for “Unix name”, prints the operative system information,

`neofetch` standalone command will display system info — like kernel version, shell, and hardware — next to an ASCII logo of d Linux distro


### `ping`, `wget`, ``unzip`:
`ping domain.com` is the networking terminal utility used to test network connectivity. ping has a ton of options, in most cases is used to hia a domain or IP address and observe returned analysis.

`wget` (World Wide Web get) is a utility to retrieve content from the internet. Usages - `wget https://url.com/filename.ext`

`unzip images.zip`