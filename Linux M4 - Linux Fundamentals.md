# Linux M4 - Linux Fundamentals

## Commands Syntax
- Commands
- Option(s)
- Argument(s)
- use ```man``` to check for command's options

## File Permissions
- Three types of permissions
    * r = read
    * w = write
    * x = execute = running a program
- Each permission (rwx) can be controlled at three levels:
    * u = user = yourself
    * g = group = can be people in the same project
    * o = other = everyone on the system
- File or Directory permission can be displayed by running ```ls -l``` command
- Command to change permission
    * ```chmod```
- You need the x (execute) permission to get in a directory (```cd```)

## Permission Using Numeric Mode

| Number | Permission Type | Symbol |
| -------- | -------- | -------- |
| 0     | No Permission     | ---     |
| 1     | Execute     | --x     |
| 2     | Write     | -w-     |
| 3     | Write+Execute     | -wx     |
| 4     | Read     | r--     |
| 5     | Read+Execute     | r-x     |
| 6     | Read+Write     | rw-     |
| 7     | Read+Write+Execute     | rwx     |

## File Ownership
- Command to change file ownership
    * ```chown <new_owner> <file>``` changes the ownership of a file
    * ```chgrp <new_group> <file>``` changes the group ownership of a file
- Recursive ownership change option (Cascade) = ```-R``` it will not only change that directory, but also all the files and directories in that parent directory

## Access Control List (ACL)
### What is ACL?
- Access control list (ACL) porvides an additional, more flexible permission mechanism for file systems. It is designed to assist with UNIX file permissions. ACL allows you to give permissions for any user or group to any disc resource
### Use of ACL
- Think of a scenario in which particular user is not a memeber of group created by youbut still you wnat to give some read or write access, how can youdo it without making user a member of group, here comes in picture Access Control Lists, ACL helps us to do this trick
- Basically, ACLs are used to make a flexible permission mechanism in Linux
- From Linux man pages, ACLs are used to define more fine-grained discretionary access rights for files and directories
- Commands to assign and remove ACL permissions are:
```
setfacl || getfacl
```
1. To add permissino for user
```
setfacl -m u:user:rwx /path/to/file
```
2. To add permissions for a group
```
setfacl -m g:group:rw /path/to/file
```
3. To allow all files of directories to inherit ACL entries from the directory it is within
```
setfacl -Rm "entry" /path/to/dir
```
4. To remove a specific entry (For a specific user)
```
setfacl -x u:user /path/to/file 
```
5. To remove all entries (For all users)
```
setfacl -b path/to/file
```
#### Note
- As you assign the ACL permissions to a file/directory it adds + sign at the end of the permission
- Setting w permission with ACL does not allow to remove a file

## Help Commands
1. ``` whatis command ```
2. ``` command --help ```
3. ``` man command ```

## TAB Completion and Up Arrow

## Adding Text to Files (Redirects)
1. ``` vi ```
2. Redirect command output ``` > or >> ```
3. ``` echo > (overwrite) or >> (append) ```

## Input and Output Redirects
1. Standard input (```stdin```) and it has fie descriptor number as ```0```
2. Standard output (```stdout```) and it has fie descriptor number as ```1```
3. Standard error (```stderr```) and it has fie descriptor number as ```2```

### Input (stdin) - 0
- Input is used when feeding file contents to a file
    * e.g. cat < listings
    * mail -s "Office memo" allusers@abc.com < memoletter

### Error (stderr) - 2
- When a command is executed we use a keyboard and that is also considered (stdin -0)
- That command output goes on the monitor and that output is (stdout -1)
- If the command produced any error on the screen then it is considered (stderr -2)
    * We can use redirects to route errors from the screen
    * e.g. ls -l /root 2> errorfile
    * e.g. telnet localhost 2> errorfile

## Standard Output to a File (tee)
- ```tee``` command is used to store and view (both at the same time) the output of any command
- The command is named after the T-spliter used in plubing. It bascially breaks the output of a program so that it can be both displayed and saved in a file. It does both the tasks simultaneously, copies the result into the specified files or variables and also display the result
```
tee -a (append)
```

## Pipes (|)
- A pipe is used by the shell to connect the output of one command directly to the input of another command
```
ls -l | more
ls -l | tail -3
```

## File Maintenance Commands
- ```cp``` copy
- ```rm``` remove
- ```mv``` move or rename
- ```mkdir``` make directory
- ```rmdir``` or ```rm -r``` remove a directory
    * ``` rm -Rf ``` will forcefully remove sub-directories and its contents as well
- ```chgrp``` change group (group level)
- ```chown``` change owner (user level)
    * ```chown user:group filename```

## File Display Commands
- ```cat```
- ```more```
- ```less```
- ```head``` (default first 10 lines)
- ```tail``` (default last 10 lines)

## Filters / Text Processors Commands
- ```cut``` allows to cut the output. It takes the unput of a command, or reading a content of a file, it takes that in as an input and then it cuts it to your desired output then
- ```awk``` allows you to list by the columns
- ```grep``` ```egrep``` for searching specifc words
- ```sort``` sorts out the output in alphabetical order
- ```uniq``` it will not show any duplicates, if there are words in the file that are the same as the one previous to it
- ```wc``` tells the count of words, letters or lines in a file or in an output

### Cut
- Cut is a command line utility that allows you to cut parts of lines form specified files or piped data and print the result to standard output. It can be used to cut parts of a line by delimiter, byte position, and character
- ```cut filename``` = Does not work
- ```cut --version```
- ```cut -c1 filename``` = List one character
- ```cut -c1,2,4 filename``` = Pick and chose character
- ```cut -c1-3 filename``` = List range of character
- ```cut -c1-3,6-8 filename``` = List specific range of characters
- ```cut -b1-3 filename``` = List by byte size
- ```cut -d: -f 6 /etc/passwd``` = List first 6th column separated by : (-d=delimiter,從哪切, -f=field,留哪個)

### awk
- awk is a utility/language designed for data extraction. Most of the time it is used to extract fields from a file or from an output
- ```awk --version```
- ```awk '{print $1}' file``` = List 1st field(column) from a file
- ```ls -l | awk '{print $1,$3}'``` = List 1st and 3rd field of ls -l output
- ```ls -l | awk '{print $NF}'``` = Last field of the output
- ```awk '/Jerry/ {print}' file``` = Search for a specific word
- ```awk -F: '{print $1}' /etc/passwd``` = Output only 1st field of /etc/passwd
- ```echo "Hello Tom" | awk '{$2="Adam"; print $0}'``` = Replace words field words
- ```cat file | awk '{$2="Jacky"; print $0}'``` = Replace words field words
- ```awk 'length($0) > 15' file``` = Get lines that have more than 15 byte size
- ```ls -l | awk '{if($9 == "seinfeld") print $0;}'``` = Get the field matching seinfeld in /home/jchen
    * $9 is the last column
- ```ls -l | awk '{print NF}'``` = Number of fields(columns)

### grep/egrep
- The grep command which stands for "global regular expression print" processes text line by line and prints any lines which made a specified pattern
- ```grep --version``` or ```grep --help```
- ```grep keyword file``` = Search for a keyword from a file
- ```grep -c keyword file``` = Search for a keyword and count
- ```grep -i keyword file``` = Search for a keyword and ignore case-sensitive
- ```grep -n keyword file``` = Display the matched lines and their line numbers
- ```grep -v keyword file``` = Display everything but keyword
- ```grep keyword file | awk '{print $1}'``` = Search for a keyword and then only give the 1st field
- ```ls -l | grep Desktop``` = Search for a keyword and then only give the 1st field
- ```egrep -i "keyword|keyword2" file``` = Search for 2 keywords

### sort/uniq
- Sort command sorts in alphabetical order
- Uniq command filters out the repeated or duplicate lines
- ``` sort/uniq --version```  or ```sort/uniq --help```
- ```sort file``` = Sort file in alphebetical order
- ```sort -r file``` = Sort in reverse alphebetical order
- ```sort -k2 file``` = Sort by field(columns) number
- ```uniq file``` = Remvoe duplicate(Every time that need to run uniq on any of the output or content file, always run sort)
- ```sort file | uniq``` = Always sort first before using uniq their line numbers
- ```sort file | uniq -c``` = Sort first then uniq and list count
- ```sort file | uniq -d``` = Only show repeated lines

### wc
- The command reads either standard input or a list of files and generates: newline count, word count, and byte count
- ```wc --version``` or ```wc --help```
- ```wc file``` = Check file line count, word count, and bytes in a file
- ```wc -l file ``` = Get the number of lines in a file
- ```wc -w file``` = Get the number of words in a file
- ```wc -c file``` = Get the number of bytes in a file
- ```wc DIRECTORY``` = NOT allowed
- ```ls -l | wc -l``` = Number of files (need to minus 1, because there is a 'total XXX')
- ```grep keyword | wc -l``` = Number of keyword lines

## Compare Files
- ```diff file1 file2``` = Compare Line by Line
- ```cmp file1 file2``` = Compare Byte by Byte

## Compress and un-Compress Files
- ```tar``` = takes a bunch of files together and put it in one container or one ifle
    * ```tar cvf file```
    * ```tar xvf file```
- ```gzip``` = actually compresses a file
- ```gzip -d``` OR ```gunzip``` = uncompress a file

## Truncate File Size (truncate)
- The Linux truncate command is often used to shrink or extend the size of a file to the specified size, it will actually chop the file to the specified size (you will lose data)
- ```truncate -s 10 filename``` = shrink file size to 10 bytes
- ```truncate -s 60 filename``` = 若要增大，只會加入空白字元^@在裡面

## Combining and Spilting Files
- Multiple files can be combined into one and;
- One file can be split into multiple files
    * cat file1 file2 file3 > file4
    * split file4
    * e.g. split -l 300 file.txt childfile
    * Split file.txt into 300 lines per file and output to childfileaa, childfileab amd childfileac

## Linux vs. Windows Commands

| Command Description | Windows | Linux |
| -------- | -------- | -------- |
| Listing of a directory     | dir     | ls -l     |
| Rename a file | ren | mv |
| Copy a file | copy | cp |
| Move file | move | mv |
| Clear screen | cls | clear |
| Delete file | del | rm |
| Compare contents of files | fc | diff |
| Search for a word/string in a file | find | grep |
| Display command help | command /? | man commnad |
| Displays your location in file system | chdir | pwd |
| Displays the time | time | date |


