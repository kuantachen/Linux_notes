# Linux M5 - System Administration

## Linux File Editor (vi)
#### A text editor is a program which enables you to create and manipulate data(text) in a Linux file
* vi = visual editor
* ed = Standard line editor
* ex = Extended line editor
* emacs = A full screen editor
* pico = Beginner's editor
* vim = Advance version of vi

## Common keys in vi
* i = get into insert mode
* Esc = Escape out of any mode
* r = (command mode) replace one character at a time
* d = delete
* :q! = quit without saving
* :wq! = quit and save
* ZZ (shift+zz) = exit and save file (escape back to command mode first)
* D = (command mode) delete the entire line
* u = (command mode) undo action
* x = (command mode) remove one character
* o = (command mode) enter to a new line and automatically get into insert mode
* / = (command mode) search for a word in vi

## How to simply use vi editor
1. Hit enter with ```vi``` command you will be in command mode not insert mode
2. Hit ```i``` to get into insert mode (you will see ``` --INSERT-- ``` at the bottom of the editor)
3. Use ```Esc``` to escape back to command mode and use ``` ZZ ``` or ``` :wq! ``` to save and quit the editor
---

## Difference between vi and vim Editors
- Which editor you choose is a matter of personal choice, both editors work in the same manner. Due to added features, learning and using vim editor is much easier than the vi editor
- Older operating system might not have vim, so we could only use vi
- vim is based on the vi, so vim has all the features as vi with some excellent addition
![](https://i.imgur.com/ugSnWoK.jpg)

## "vim" Interactive Learning Tools
- https://www.openvim.com/
- http://www.vimgenius.com
- https://www.vim-adventures.com/

## "sed" Command
- Replace a string in a file with a newstring
- Find and delete a line
- Remove empty lines
- Remove the first or n lines in a file
- To replace tabs with spaces
- Show defined lines from a file
- Substitute within vi editor
- And more......
```
# Only replace in the output(screen), not the file (s for substitute; g for apply it globally)
sed 's/Kenny/Lenny/g' seinfeld-characters

# Insert the changes in to the file
sed -i 's/Kenny/Lenny/g' seinfeld-characters

# Remove particular word
sed 's/Costanza//g' seinfeld-characters

# Delete the particular word (d for delete)
sed '/Seinfeld/d' seinfeld-characters

# Delete anything that starts and end with nothing (nothing in the middle = empty line)
sed '/^$/d' seinfeld-characters

# Delete the 1st line
sed '1d' seinfeld-characters

# Replace tab with space
sed 's/\t/ /g' seinfeld-characters

# Show only lines 12-18
sed -n 12-18p seinfeld-characters

# Show lines expected 12-18
sed 12,18d seinfeld-characters

# Add empty line in every record
sed G seinfeld-characters

# Replace Seinfeld with S except the eighth line Seinfeld
sed '8!s/Seinfeld/S/g' seinfeld-characters
# In vi editor
(command mode) %s/Seinfeld/S
```


## User Account Management
### Commands
1. ```useradd```
2. ```groupadd```
3. ```userdel```
4. ```groupdel```
5. ```usermod```
```
useradd -g superheros -s /bin/bash -c "user description" -m -d /home/spiderman spiderman 
```
```
-g : add a gorup
-s : give a shell environment
-m : --move-home
-d : use a home directory
```
### Files
1. /etc/passwd
2. /etc/group
3. /etc/shadow

## Create Password
```passwd <username>```

## Enable Password Aging
### The /etc/login.defs File
- Chage command is use per user based
- e.g. ```chage [-m mindays] [-d lastday] [-I inactive] [-E expiredate] [-W warndays] user```
- File = ```/etc/login.defs```
- ```-d``` = Last password change: days since Jan1, 1970 that password was last changed
- ```-m``` = Minimum: the minimum number of days required between password changes
- ```-M``` = Maximum: the maximum number of days the password is valid (after that user is forced to change password)
- ```-W``` = Warn: the number of days before password is to expire that user is warned that password must be changed
- ```-I``` = Inactive: the number of days after password expires that account is disabled
- ```-E``` = Expire: days since Jan1, 1970 that account is disabled
```
# 密碼最少要使用5天，超過90天要換密碼，即將到期的前10天會發出警告，密碼過期後該帳戶3天候會被禁用
chage -m 5 -M 90 -W 10 -I 3 babubutt

grep babubutt /etc/shadow
```
![](https://i.imgur.com/tb89tBV.png)

## Switch Users and sudo Access
- ```su - username```
- ```sudo command``` = if you do not have root privileges or you cannot become root, but you still need to run certain commands
    * e.g. ```dmidecode``` ```fdisk -l```
- ```visudo``` = edits the ```/etc/sudoers``` which is a configuration file that allows users to add or remove the rights to run certain commands
- Add a user to become a powerful user (add user to wheel group, so that the user could use ```sudo command```)(user in wheel group are allowed to run all commands)
```
# Add user to sudoers group (group level)
usermod -aG wheel jchen

grep wheel /etc/group
```

## Monitor Users
- ```who```
- ```last```
    * ```last | awk '{print $1}' | sort | uniq```
- ```w```
- ```finger```
- ```id```

## Talking to Users
- ```users```
- ```wall``` = broadcast your message to everyone who is logged into the Linux system
    * after typing the message use ```Ctrl+d``` to send it out
- ```write``` = dedicated message to a specific user
    * immediately send the message to the user you specify (active)

## Linux Directory Service - Account Authentication
- Types of accounts
    * Local accounts
    * Domain/Directory accounts
- Windows = Actice Directory
- LDAP(Lightweight Directory Assistance Protocal) is just a protocol, it is use to authenticate against a directory

## Difference between Active Directoyr, LDAP, IDM, WinBIND, OpenLDAP etc.
- Active Directory = Microsoft
- IDM = Identity Manager
- WinBIND = Used in Linux to communicate with Windows (Samba). If you have Active Directory users and they wanted to log into Linux machine, they cannot because Windows only talk to Windows, so Samba is a lind of communicator messenger in the middle, which allow Windows users or Windows Active Directory users to log into Linux machines using WinBIND
- OpenLDAP (open source)
- IDM Directory Server
- JumpCloud
- LDAP = Lightweight Directory Access Protocol

## System Utility Commands
- ```date```
- ```uptime``` = current Linux system time, how long the system has been running, numbers of users currently logged into your Linux system, the average CPU load (average number of jobs in your system's run queue) for the 1, 5, and 15 minutes
- ```hostname```
- ```uname```
- ```which``` = the location that your command run
- ```cal``` = calendar of this month
    * ```cal 9 1997``` ```cal 2016```
- ```bc``` = binary calculator (```quit``` to leave)

## Processes and Jobs
- Application = Service: like a program that runs in your computer
- Script = something that is written in a file and then packaged in a way it could execute. Shell scriptes or Commands are list of instructinos, e.g. adduser, cd, pwd, etc.
- Process = when a application runs or start up, it actually generates a process (process ID associated to that application)
- Daemon = something that continuously runs in the background or it doesn't stop
- Threads = every process could have multiple threads associated with it
- Job or Workorder = something that is created by scheduler , run a service or process at a schedule time

## Process / Services Commands
- ```systemctl```
- ```ps``` = allows to see what are the processes running in your Linux system
- ```top``` = see all the processes based on its load, also tells memory information, CPU information that is being used by that process
- ```kill``` = kills the process by its name or process ID
- ```crontab``` = used to schedule applications or processes or services
    * ```crontab -e``` to edit it
    * when process or application is scheduled in crontab it becomes a job
- ```at``` = one time basis, like crontab

### systemctl command
- systemctl command in a new tool to control system services
- ```systemctl start|stop|status servicename.service```

- ```systemctl enable|disable servicename.service``` = start the application when Linux starts at boot

- ```systemctl restart|reload servicename.service```

- ```systemctl list-units --all``` = see what are the list of all the service units that are available
    * UNIT: the ```systemctl``` unit name
    * LOAD: whether the unit's configuration has been parsed by ```systemctl```. The configuration of loaded units is kept in memory
    * ACTIVE: a summary state about whether the unit is active. This is usually a fairly basic way to tell if the unit has started successfully or not
    * SUB: This is a lower-level state that indicates more detailed information about the unit. This often varies by unit type, state, and the actual method in which the unit runs
    * DESCRIPTION: a short textual description of what the unit it/does
- To add a service under systemctl management:
    * Create a unit file in ```/etc/systemd/system/servicename.service```
- To control system with ```systemctl```
    * ```systemctl poweroff```
    * ```systemctl halt```
    * ```systemctl reboot```


### ps command
- ps command stands for process status and it displays all the currently running process in the Linux system
- ```ps``` = shows the processes of the current shell
    * PID = the unique process ID
    * TTY = terminal type that the user logged into
    * TIME = amount of CPU in minutes and seconds that the process has been running
    * CMD = name of the command
- ```ps -e``` = shows all running processes
- ```ps aux``` = shows all running processes in BSD format (Berkeley system's description)
- ```ps -ef``` = shows all running processes in full format listing (Most commonly used)
- ```ps -u username``` = shows all processes by username

### top command
- top command is used to show the Linux processes and it provides a real-time view of the running system
- This command shows the summary information of the system and the lsit of processes or threads which are currently managed by the Linux Kernal
- When the top command is executed then it goes into interactive mode and you can exit out by hiting ```q```
- ```top```
    * PID = shows task's unique process id
    * USER = username of owner of task
    * PR = the 'PR' feild shows the sceduling priorty of the process from the perspective of the kernel
    * NI = represents a Nice Value of task. A Negative nice value implies higher priority, and positive Nice value means lower priority
    * VIRT = total virtual memory used by the task
    * RES = memory consumed by the process in RAM
    * SHR = represents the amount of shared memory used by a task
    * S = this field shows the process state in the single-letter form
    * %CPU = CPU usage
    * %MEM = memory usage of task
    * TIME+ = CPU time, the same as TIME, but reflecting more granularity through hundredths of second
- ```top -u username``` = show tasks/processes by user owned
- ```top```then press```c``` = show commands absolute path
- ```top```then press```k``` = kill a process by PID with top session
- ```top```then press```M```&```P``` = to sort all Linux running processes by Memory usage

### kill command
- ```kill``` command is used to terminate processes manually
- It sends a signal which ultimately terminates or kills a particular process or group of processes
- ```kill [option] [PID]```
    * OPTION = signal name or signal number/ID
    * PID = process ID
- ```kill -l``` = to get a list of all signal names or signal number
    * ```kill PID``` = kill a process with default signal
    * ```kill -1``` = restart
    * ```kill -2``` = interrupt from the keyboard just like Ctrl C
    * ```kill -9``` = forcefully kill the process
    * ```kill -15``` = kill a process gracefully
- Other similar kill commands are:
    * ```killall``` = kill all the processes and it relate child processes
    *  ```pkill``` = when the process do not have a PID, kill by its name

### crontab command
- ```crontab``` is used to schedule tasks
    * ```crontab -e``` = edit the crontab (will automatically get in a vi editor)
        * 分 時 日 月 星期幾 command to execute
    * ```crontab -l``` = list the crontab entries
    * ```crontab -r``` = remove the crontab
    * ```crond``` = crontab daemon/service that manages scheduling
    * ```systemctl status crond``` = to manage the crond service
![](https://i.imgur.com/JKJLSJT.png)

- Create crontab entry by scheduling a task:
```
crontab -e

schedule, echo "This is my first crontab entry" > crontab-entry

48 22 * 2 * echo "This is my first crontab entry" > crontab-entry
(在2月每天的22:48 做後面的指令)
```

### at command
- ```at``` is like crontab which aloows you to schedule jobs but only once
- When the command is run it will enter interactice mode and you can get out by pressing ```Ctrl D```
- ```at HH:MM PM/AM``` = schedule a job
- ```atq``` = list the at entries
- ```atrm #(number)``` = remove at entry
- ```atd``` = to manage the atd service
- Create at entry by scheduling a task:
```
at 4:45PM -> enter
echo "This is my first at entry" > at-entry
-> Ctrl D
```
- Other future scheduling format:
    * ```at 2:45 AM 101621``` = schedule a job to run on Oct.16 2021 at 2:45am
    * ```at 4PM + 4 days``` = schedule a job at 4pm 4 days from now
    * ``` at now +5 hours``` = schedule a job to run 5 hours from now
    * ```at 8:00 AM Sun``` = schedule a job to 8am on coming Sunday
    * ```at 10:00 AM next month``` = schedule a job to 10am next month


## Additional Cron Jobs
- Default types of cronjobs
    * Hourly
    * Daily
    * Weekly
    * Monthly
- All the above crons are setup in
    * ```/etc/cron.___ (directory)```
- The timing for each are set in
    * ```/etc/anacrontab -- (except hourly)```
- For hourly
    * ```/etc/cron.d/0hourly```
- Move your scripts in the above directory

## Process Management
- Background = ```Ctrl-z```(stopped), ```jobs```(check jobs) and ```bg```(throw to background and run) 
- Foreground = ```fg``` (throw back to foreground and run)
- Run process even after exit = ```nohup process &``` OR ```nohup process > /dev/null 2>&1 &```
- Kill a process by name = ```pkill processname```
- Process priority = ```nice (e.g. nice -n 5 process)```
    * The niceness scale goes from -20 to 19. The lower the number more priority that task gets
- Process monitoring = ```top```
- List process = ```ps```

## System Monitoring Commands
- ```top```
- ```df``` = report file system disk space usage
- ```dmesg``` = gives you the output of the system, related to warnings, error messages, failures, something related to system hardware etc.
- ```iostat 1``` = (input and output statistics) 1 is update every 1 second
- ```netstat -rnv``` = gateway information etc.
- ```free``` = physical memory, swap space which is virtual memory utilization
- ```cat /proc/cpunifo```
- ```cat /proc/meminfo```

## System Log Monitoring
- Log Directory = ```/var/log```
    * ```boot``` = logs when system is boot up or reboot
    * ```chronyd = NTP```
    * ```cron```
    * ```maillog```
    * ```secure```
    * ```messages``` = monitor system activities (almost everything is here)
    * ```httpd``` = apache application log

## System Maintenance Commands
- ```shutdown```
- ```init 0-6``` = 0:simply shutdown, 6:reboot, 3:bring it in multiuser mode
- ```reboot```
- ```halt``` = shutdown right away

## Changing System Hostname
```
hostname set-hostname newhostname
```
- Version 7 = edit /etc/hostname
- Version 6 = edit /etc/sysconfig/network

## Finding System Information
```
cat /etc/redhat-release

uname -a

dmidecode (root)
```

## Finding System Architecture
- Difference between a 32-bit and 64-bit CPU
- A big difference between 32-bit processors and 64-bit processors is the number of calculations per second they can perform
- Linux = ```arch``` or ```uname -a```
- Windows = My Computer -> Properties

## Terminal Control Keys
- ```CTRL-u``` = erase everything you've typed on the command line
- ```CTRL-c``` = stop/kill a command
- ```CTRL-z``` = suspend a command
- ```CTRL-d``` = exit from an interactive program (signals end of data e.g. ```at```)

## Terminal Commands
- ```clear``` = clear your screen
- ```exit``` = exit out of the shell, terminal or a user session
- ```script``` = the script command stores terminal activites in a log file that can be named by a user, when a name is not provided by a user, the default file name, typescript is used (```exit``` could that the script done)

## Recover Root Password (single user mode)
1. Restart your computer
    * 登入介面按e
    * 找到 ro -> 輸入 rw init=/sysroot/bin/sh
    * Ctrl-x -> 會啟動虛擬機in single user mode
    * chroot /sysroot
    * passwd root 重設密碼
    * 輸入 touch /.autorelabel
    * exit
    * reboot
3. Edit grub
4. Change password
5. Reboot

## SOS Report
- What is SOS Report?
    * Collect and package diagnostic and support data
- Package name
    * sos-version
- Command
    * ```sosreport``` (root)

## Environment Variables
- What are environment variables?
    * An environment vairable is a dynamic-named value that can affect the way running processes will behave on a computer. They are part of the environment in which a process runs
    * In simple words: set of defined rules and values to build an environment
- To view all environment varialbes
    * ```printenv``` OR ```env```
- To view ONE environment variable
    * ```echo $SHELL```
- To set the environment variables
    * ```export TEST=1```
    * ```echo $TEST```
- To set environment variable permanently
```
vi .bashrc
TEST='123'

export TEST
```
- To set global environment variable permanently
```
vi /etc/profile OR /etc/bashrc
TEST='123'

export TEST
```

## Special Permissions with setuid, setgid and sticky bit
- All permissions on a file or directory are referred as bits
- There are 3 additional permissions in Linux
    * setuid: bit tells Linux to run a program with the effective user id of the owner instead of the executor (e.g. ```passwd```) -> /etc/shadow
    * setgid: bit tells Linux to run a program with the effective group id of the owner instead of the executor (e.g. ```locate``` or ```wall``` ) --> Note: This bit is present for only files which have executable permissions
    * sticky bit: a bit set on files/directories that allows only the owner or root to delete those files
- To assign special permissions at the user or group level
    * ```chmod u+s xyz.sh```
    * ```chmod g+s xyz.sh```
- To remove special permissions at the user or group level
    * ```chmod u-s xyz.sh```
    * ```chmod g-s xyz.sh```
- To find all executables in Linux with setuid and setgid permissions
    * ```find / -perm /6000 -type f```
- Please note: These bits only work on c programming executables not on bash shell scripts

### Sticky bit
- It is assigned to the last bit of permissions
    * ```rwxrwxrwt```
    * e.g. ```ls -l /tmp```
