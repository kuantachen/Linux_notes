# Linux M6 - Shell Scripting

## Linux Kernel
- What is a Kernel?
    * Interface between hardware and software
    * A kernel is a program that is stored inside of your operating system, it takes command from a shell
    * Shell: GUI, bash, csh; we executed commands here and commands are forwarded to kernel, and it's kernel's responsibility to talk to the hardware
    * Operating System = Kernel + Shell
    * Software = Shell + Application
    ![](https://i.imgur.com/LVWXkGM.png)
    
## Introduction to Shell
- What is a Shell?
    * Its like a container
    * Interface between users and Kernel/OS
    * CLI is a Shell
- Find your Shell
    * ```echo $0```
    * Available Shells ```cat /etc/shells```
    * My Shell -> ```/etc/passwd```
- Windows GUI is a shell
- Linux KDE GUI is a shell
- Linux sh, bash etc. is a shell

## Types of Linux Shells
1. Gnome
    * A graphical environment in Linux
2. KDE
3. sh (Bourne Shell)
    * one of the original shell, developed by Unix computers by Stephan Bourne
4. bash (Born Again Shell)
    * user friendly, lots of enhanced features
    * Most of the Linux use bash
5. csh and tcsh
    * using C Syntex as a model
    * tsch does not run bash scripts
6. ksh
    * is compatible with sh and bash
    * improves from sh by adding floating point...
    * usually used in Solaris

## Shell Scripting
- What is a Shell Script?
    * A shell script is an executable file containing multiple shell commands that are executed sequentially. The file can contain:
    1. Shell (```#!/bin/bash```)
    2. Comments(```# comments```)
    3. Commands(```echo, cp, grep etc.```)
    4. Statements(```if, while, for etc.```)
- Shell script should have executable permissions (e.g. ```-rwxr-xr-x```)
- Shell script has to be called from absolute path (e.g. ```/home/userdir/script.bash```)
- If called from current location then ```./script.bash```

## Basic Shell Scripts
- Design my own scripts:
    * Output to screen using ```echo```
    * Createing tasks:
        * Telling your id, current, location, your files/directories, system info
        * Creating files or directories
        * Output to a file ">"
- Filters/Text processors through scripts ```cut, awk, grep, sort, uniq, wc```

## Input and Output of Script
- Create script to take input from the user
    * ```read``` = take input from user
    * ```echo``` = output
```
#!/bin/bash
# Author
# Date
# Description

echo Hello, my name is Jacky Chen
echo
echo What is your name?
read namecont
echo
echo Hello $namecont
echo

a=`hostname`    # commmand 要加``
echo Hello, my server name is $hostname
echo
echo What is your name?
read b
echo
echo Hello $b
echo
```

## if-then scripts
```
#!/bin/bash

count=100
if [ $count -eq 100 ]
  echo Count is 100
else
  echo Count is not 100
fi
```
```
#!/bin/bash

clear
if [ -e /home/jchen/error.txt ]    # -e: check something if it exist
then
    echo "File exist"
else
    echo "File does not exist"
fi
```

## for loops scripts
```
#!/bin/bash

for i in 1 2 3 4 5
do
    echo "Welcome $1 times"
done
```
```
#!/bin/bash

for i in eat run jump play
do
    echo See Jacky $i
done
```

## do-while scripts
- The while statement continually executes a block of statements while a particular condition is true or met
```
#!/bin/bash

while [ condition ]
do
    command1
    command2
    command3
done
```
```
#!/bin/bash

count=0
num=10
while [ $count -lt 10 ]    # -lt = less than
do
    echo
    echo $num seconds left to stop this process $1
    echo
    sleep 1

num=`expr $num - 1`    # expr = expression 一個計數器
count=`expr $count + 1`
done
echo
echo $1 process is stopped!!!
ech0

# In this case $1 will be the name of the process or process ID entered after running the script. e.g. script-dowhile 25046
```

## Case Statement scripts
- Most of the installation programs are written in case statements where the scripts waits for user input to select from choices
```
#!/bin/bash

echo
echo Please chose one of the options below
echo
echo 'a = Display Date and Time'
echo 'b = List file and directories'
echo 'c = List users logged in'
echo 'd = Check System uptime'
echo
    read choices
    
    case $choices in

a) date;;
b) ls;;
c) who;;
d) uptime;;
*) echo Invalid choice - Bye.    # any other options

    esac

```

## Check Remote Servers Connectivity
- A script to check the status of remote hosts
```
#!/bin/bash

ping -c1 192.168.1.1
    if [ $? -eq 0 ]
    then
    echo OK
    else
    echo NOT OK
    fi


---


# Don't show the output
#!/bin/bash

ping -c1 192.168.1.1 &> /dev/null
    if [ $? -eq 0 ]
    then
    echo OK
    else
    echo NOT OK
    fi


---

   
# Define Variable
#!/bin/bash

hosts="192.168.40.158"
ping -c1 $hosts &> /dev/null
    if [ $? -eq 0 ]
    then
    echo $hosts is OK
    else
    echo $hosts is NOT OK
    fi
```

- ```$?``` is the output of all the command that you run, by default the ```$?``` returns 0/1 which is that command has successfully/unsuccessfully run ($? = return value)

- Pinging Multiple IPs:
    * Create a IP list file first (myhosts)
```
#!/bin/bash

hosts="/home/jchen/pro-scripts/myhosts"

for ip in $(cat $hosts)
do
    ping -c1 $ip &> /dev/null
    if [ $? -eq 0 ]
    then
    echo $ip is OK
    else
    echo $ip is NOT OK
    fi
done

```

## Aliases
- Aliases is a very popular command that is used to cut down on lengthy and repetitive commands
```
alias ls="ls -al"
alias pl="pwd; ls"
alias tell="whoami; hostname; pwd"
alias dir="ls -l | grep ^d"
alias lmar="ls -l | grep Mar"
alias wpa="chmod a+w"
alias d="df -h | awk '{print \$6}' | cut -c1-4"
```
- ```alias``` = could see all the alias that are created
- ```unalias <the alias name>``` = delete the alias

## Creating User or Global Aliases
- When the alias is create in your own session it will be remove when you logout your session
- User Alias = Applies only to a specific user profile
    * User = /home/user/.bashrc
```
vi .bashrc

# Personal Aliases
alias hh="hostname"
```
- Global Alias = Applies to everyone who has account on the system
    * Global = /etc/bashrc
```
vi /etc/bashrc

# Global Aliases
alias hh="hostname"
```

## Shell History
- All commands are recorded, very effective command for system administrators to troubleshoot
- ```history```
- ```!<historynumber>```
- The file where history of your shell commands saved = /home/yourname/.bash_history
- To view other users history of shell commands
    * Become root (```su -```)
    * ```cat /home/users-dir-name/.bash_history```

