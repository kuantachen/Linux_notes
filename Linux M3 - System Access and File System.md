# Linux M3 - System Access and File System

## Prompt Synmbols
1. #: root
2. $: anyuser
3. Ctrl+C: To get your prompt back

## Get Username/Host
```
whoami
```
```
hostname
```

## Access to Linux System
1. Window -> Remote Desktop (RDP)
2. VMware ESX -> vSphere client
3. Linux -> Putty, SecureCRT, SSH from Linux to Linux

## New Network Command (IP)
1. CentOS/RHEL 5 or 6 = ```ifconfig```
2. CentOS/RHEL 7 = ```ip```
3. CentOS/RHEL 7.5 and up = ```ifconfig``` command have been deprecated
4. use ```ip addr```

## Download/Connect Putty
https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.76-installer.msi

## Important Things to Remember in Linux
1. Linux has super-user account called root
2. Linux is case-sensitive system
3. Avoid using spaces when creating files and directories
4. Linux kernel is not a operating system. It is a small software within Linux operating system that takes commands from users and pass them to system hardware or peripherals
5. Linux is mostly CLI(Command Line Interface) not GUI(Graphical User Interface)
6. Linux is very flexible as compared to other operating systems

## File System Structure and its Descroption
- /boot: Contains file that is used by the boot loader (grub.cfg)
- /root: root user home directory. It is not same as /
- /dev: System devices (e.g. disk, cdrom, speakers, flashdrive, keyboard etc.)
- /etc: Configuration files
- /bin -> /usr/bin: Everyday user commands
- /sbin -> /usr/sbin: System/filesystem commands
- /opt: Optional add-on applications (Not part of OS apps)
-  /proc: Running porcesses (Only exist in Memory)
-  /lib -> /usr/lib: C programming library files needed by commands and apps
    * ``` strace -e open pwd ```
- /tmp: Directory for temporary files
- /home: Directory for user
- /var: System logs
- /run: System daemons that start very early (e.g. systemd and udev) to store temporary runtime files like PID files
- /mnt: To mount external filesystem (e.g. NFS)
- /media: For cdrom mounts

## Navigating File System
- ```cd``` : change directory
- ```pwd``` : print working directory
- ```ls``` : lists, list all the directories/files within a current working directory
    * -ltr: list file and directory in timely order and in reverse, meaning the oldest one at the top

## File System Paths
- Absolute Path: always begins with a "/". This indicates that the path starts at the root directory
- Relative Path: does not begin with a "/". It identifies a location relative to your current position

## Creating Files and Directory
- Creating Files:
    * touch
    * cp
    * vi
- Creating Directorys:
    * mkdir

## Coping Directories (```cp```)
- To copy a directory on Linux, you have to execute the ```"cp"``` command with ```"-R"``` option for recursive and specify the source and destination directories to be copied
    * cp -R <soucre_folder> <destination_folder>

## Linux File Types
1. - = Regular file
2. d = Directory
3. l = Link
4. c = Special file or device file
5. s = Socket
6. p = Named pipe
7. b = Block device

## Find Files and Directories
- Commands are used to find files/directories
    * ```find```
    * ```locate```
- ```locate``` uses a prebuilt database, which should bu regularly updated
- ```find``` iterates over a filesystem to locate files
- Thus, locate is much faster than find, but can be inaccurate if the database (can be seen as a cache) is not updated
- To update locate database run ```updatedb``` (root)

## Changing Password
- ```passwd userid``` (root)
- if you are user and you want to change your own password, use ```passwd```

## WildCards
- A wildcard is a character that can be used as a substitute for any of a class of characters in a search
    * \* = represents zero or more characters
    * ? = represents a single characters
    * [] = represents a range of characters
    * ```touch abcd{1..9}-xyz``` create 9 files
    * \ = (slash) as an escape character
    * ^ = (caret) the beginning of the line
    * $ = (dollar sign) the end of the line

## Soft and Hard Links
- inode = Pointer or number of a file on the hard disk (有點像檔案的ID)
    * ```ls -ltri <file_name>```  shows the inode of the file
- Soft Link = Link will be removed if file is removed or renamed
- Hard Link = Deleting renaming or moving the original file will not affect the hard link
- Create hard link:
    * ```ln```
- Create soft link:
    * ```ln -s <source file>``` (type command in the destination directory)
- You cannot create soft or hard link within the same directory with the same name. That is why we will create links in ```/tmp``` directory