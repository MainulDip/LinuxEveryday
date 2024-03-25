### Linux Terms:
rc - stands for `run commands`
anything.d - `.d` at the end is a `directory` notation

### `source file` command:
It is used to execute a script file in current shell environment. Which updates the shell session with updated environment thats being executed from the shell file. 

The file is typically a shell script, but it can also be any text file containing a series of commands. source command is often used to set environment variables, define functions, and execute initialization scripts.

### `alias customName="linux-command"` :
It defines temporary aliases for current shell session.
ex, `alias ls="ls --color=auto"` will make ls as the mentioned command alias

To make it permanent, the alias need to be put inside `~/.bash_aliases` or `~/.bashrc` file. After that, from next login/boot the aliases will be tied with the shell. To load aliases immediate to current shell, `source ~/.bash-aliases` needs to be run or the .bashrc.