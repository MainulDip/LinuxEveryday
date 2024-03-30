### Shell Script File
```sh
! /bin/bash # telling 
echo Hello World
```
save the file as simple.sh and run by `bash simple.sh`.
to run a .sh file, an interpreter is required, ie bash. To see the interpreter, run `echo $SHELL`. Usually it'll be `/bin/bash`

### Adding Interpreter inside shell script with `#!` She-bang:
When bash interpreter is already set in the script, and the gets execution permission by `chmod u+x file`, the file can be run directly from terminal without specifying bash interpreter, ie, `./shellfile.sh`
```sh
#!/bin/bash # specifying a shell interpreter using `#!` she-bang
echo Hello World
```

### Bash Script With User Inputs:
```
#!/bin/bash

echo What is your first name?
read FIRST_NAME # will wait for user input and will assign to FIRST_NAME variable

echo Last Name?
read LAST_NAME

echo Your Full Name is $FIRST_NAME $LAST_NAME 
```

### Positional augments:
Its like `argument by/in position separated by space`. ie, `echo hello world!`, here `hello` is position 1 and `world!` is position 2. Position 0 is reserved for bash itself.

Here is the file.sh content. When executing this script, arguments can be injected using `./file.sh Hello World!` shell cmd
```sh
#!/bin/bash # file.sh
echo $1 $2
``` 
### Pipe, Greater-than and Double Greater-than for Output Redirecting:
pipe => `ls -la | grep filename`, here output of first command is redirected to used by `grep` command
greater-than `>` : is used to redirect output/result to a newFile. The file will be created on the fly
double-grater-than `>>` : to append some output to a file
```sh
echo Hello World! > greeting.txt 
```


### Single `<`, Double `<< EOF` & Triple less-than `<<<` and input source for a command
`wc -w < file.txt`: word count input from file.txt

`<< EOF` is used for multiline content, and 
```sh
cat << EOF
Hello
World!
World!!
EOF
```
Triple `<<<` is used to supply `string` input for the command
`wc -w <<< "Hello World!"`, note double quote is obligatory 

### Exit Codes and Test Expression `[...]`:
There are few exit codes which are returned if the command ran successfully or not. `echo $?` will provide exit code for the previously ran command.

0 - Successful execution
1 - Catch generic errors (“Divide by zero”, “missing operand”, “Permission denied”)
2 - Improper command usage (“Missing keyword”, “No such file or directory”)
127 - The issue in path resolution 
130 - Fatal error
255 - Out of range

Testing an expression can be made by wrapping statement by square bracket `[]`

```sh
[ hello = hello ] \
echo $? // will return 0 as true

[ 1 -eq 2 ] \
echo $?
```

### if/elif/else/fi:
`{1,,}` here 1 is used for positional argument and double comma here is a parameter expansion which ignore case sensitivity

* file.sh
```sh
#!/bin/bash

if [${1,,} = Alex ]; then
    echo "hi Alex"
elif {${1,,} = SomeoneElse}; then
    echo "Then Who Are You"
else 
    echo "Good Bye"
fi # ending if clause
```

Test the file by running `./file.sh Alex`, where Alex is the positional argument


### Function, Returns and Exit Code:
```sh
#!/bin/bash

someFunctionDefinition(){
    echo hello $1
    if [ ${1,,} = Alex ]; then
        return 0
    else
        return 1
    fi
}

someFunctionDefinition $1 # calling the function

if [ ${1,,} = 1 ]; then
    echo "You're not Alex"
fi

```
Test the file by running `./file.sh Alex`, where Alex is the positional argument

### Sequential and Multiline Commands:
For sequential bash command,
- a separate shell script can utilize multi line commands by default
- bash script on both terminal and script file can use `;` in between command to separate them
- `&&` can be used everywhere

For using multiline bash commands in terminal
- black-slash `\` followed by newline without any space before the newline
- `&&` can also be used

```sh
# using `;`
long ; medium ; short

# using `&&`
long && medium && short

# multiline
long \
medium &&
short
```

### file inspection and permission `-rwx` | ls -l and `chmod`:
The first block tells if something is a directory (`d`) or a file (`-`). After file/directory marking, there are 3 sections (owner/u, group/g, other/o), each containing 3 character of `rwx-`

- r (read)
- w (write)
- x (execute)

ie, if a file states `-rwxrw-r--`, that means
`-` : First dash telling its a file
`rws` : is telling the owner/user (`u`) of this file has read, write and execution permission
`rw-` : is telling, the group (`g`) this file belongs to, have read/write and no execution permission
`r--` : the last bits of characters telling, others (`o`) have only read permission

* Changing file permission | `chmod <u/g/o><-/+><r/w/x> <fileName/directoryName>`
ie, if a file is `-rwxrw-r--`, and we want to give write permission to others, the command will be `chmod o+w fileName`

* if u/g/o is not mentioned, ie, `chmod +x script.sh` this will modify the file with executable permission for all users (u,g,o all) 

### file permissions using numbers | `chmod 754 fileOrDirName` :
If 3 number (instead of letter) are supplied after chmod command, each representing permissions for `user`, `group`, `other` sections sequentially

Also, in file permission, 4,2,1,0 numbers represents special meaning

`4` - stands for `read`
`2` - stands for `write`
`1` - stands for `execute`
`0` - stands for `no permission`

* each section's permission is the sum up value of these numbers

ie, `chmod 764 someFileName` represents `-rwxrw-r--`
- 1st position 7 as permission for user/owner of the file, where 7 = 4 + 2 + 1, or all permission

- 2nd position 6 as permission for user/owner of the file, where 6 = 4 + 2, or read and write permission

- 3rd position as permission for other, where 4 = 4, or only read permission

### Chmod using `chmod <u/g/o><+/-><rwx> file`:
File permission can also be modified like `chmod u+x file` which gives user/owner execution permission.