### Shell Script File
```sh
! /bin/bash # telling 
echo Hello World
```
save the file as simple.sh and run by `bash simple.sh`.
to run a .sh file, an interpreter is required, ie bash. To see the interpreter, run `echo $SHELL`. Usually it'll be `/bin/bash`

### Adding Interpreter inside shell script
```sh
! /bin/bash # specifying a shell interpreter
echo Hello World
```
### file inspection and permission `-rwx` | ls -l:
The first block tells if something is a directory (`d`) or a file (`-`). After file/directory marking, there are 3 sections (owner/u, group/g, other/o), each containing 3 character of `rwx-`. Where r/w read/write, x for executable , and `-` for none

ie, if a file states `-rwxrw-r--`, that means
`-` : First dash telling its a file
`rws` : is telling the owner/user (`u`) of this file has read, write and execution permission
`rw-` : is telling, the group (`g`) this file belongs to, have read/write and no execution permission
`r--` : the last bits of characters telling, others (`o`) have only read permission

* Changing file permission | `chmod <u/g/o><-/+><r/w/x> <fileName/directoryName>`
ie, if a file is `-rwxrw-r--`, and we want to give write permission to others, the command will be `chmod o+w fileName`

### file permissions using numbers | `chmod 754 fileOrDirName` :
If 3 number (instead of letter) are supplied after chmod command, each representing permissions for `user`, `group`, `other` sections sequentially

Also, in file permission, 4,2,1,0 numbers represents special meaning

`4` - stands for `read`
`2` - stands for `write`
`1` - stands for `execute`
`0` - stands for `no permission`

* each section's permission is the sum up value of these numbers

ie, `chmod 764 someFileName` represents
- 7 as permission for user/owner of the file, where 7 = 4 + 2 + 1, or all permission

- 7 as permission for user/owner of the file, where 6 = 4 + 2 + 0, or read and write permission

- 4 as permission for other, where 4 = 4 + 0 + 0, or only read permission