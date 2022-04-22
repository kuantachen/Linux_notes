# Linux M7 - Networking, Services, and System Updates

## Internet Access to VM
1. Open Virtualbox Manager
2. Select the machine you cannot get internet on in the left pane
3. Click the Settings button in the top menu
4. Click Network in the left pane in the settings window
5. Switched to Bridge Adaptor in the Attached to drop-down menu
6. Hit OK to save your changes
7. Start your VM
- 要開瀏覽器進行網路驗證(公司網路)
- Command line 網路驗證(待確認): ```curl -XPOST -d 'username=xxx' -d 'password=xxx' -d 'action=login' "http://172.16.100.1/dhcpcfg/login6.pl"```

## Network Componenets
- IP (Internet Protocol): a unique string of numbers seperated by periods that identifies each computer using the Internet Protocol to communicate over a network.
- Subnet mask: is a 32-bit number that masks an IP address, and divides the IP address into network address and host address. It is made by setting network bits to all "1"s and setting host bits to all "0"s
- Gateway: tells your computer which route you have to pick to send your traffic out and to receive your traffic in
- Static vs. DHCP (浮動&固定IP)
- Interface
- Interface MAC

## Network Files and Commands
- Interface Detection
- Assigning an IP address
- Interface configuration files:
    * /etc/nsswitch.conf = which tells the system where is it should resolve its hostname to IP address
    * /etc/hostname
    * /etc/sysconfig/network
    * /etc/sysconfig/network-scripts/ifcfg-nic
        * ifcfg-enp0s3 -> Interface
    * /etc/resolv.conf = specifies your DNS server, DNS server is a server actually resolve hostname to IP, IP to hostname, and hostname to hostname
- Network Commands:
    * ```ping```
    * ```ifconfig```
    * ```ifup``` OR ```ifdown``` = bring up/down a network interface
    * ```netstat -rnv``` = tells the gateway, how your traffic is flowing
    * ```tcpdump -i <interface>``` = traces every single transactions that are leaving your machine and coming into your machine

## NIC Information
- NIC = Network Interface Card
- NIC can have multiple ports. Most of the time port is incorrectly reffered as NIC in IT
    * e.g. ```ethtool enp0s3```
- Other NICs:
    * lo = the loopback device is a special interface that your computer uses to communicate with itself. It is used mainly for giagnostics and troubleshooting, and to connect to servers running on the local machine
    * virb0 = the virbr0, or "Virtual Bridge 0" interface is used for NAT (Network Address Translation). Virtual environments sometimes use it to connect to the outside network

## NIC or Port Bonding
- NIC (Network Interface Card) bonding is also known as Network bonding. it can be defined as the aggregation or combination of multiple NIC into a single bond interface
- It's main purpose is to provide high availability and redundancy
### NIC Bonding Procedurce
```
modprobe bonding

modinfo bonding

Create:
/etc/sysconfig/network-scripts/ifcfg-bond0
# BOOTPROTO = none OR static if you want to assign a static IP address
# ONBOOT = yes, To enable when system reboots

Edit:
/etc/sysconfig/network-scripts/ethernet1
/etc/sysconfig/network-scripts/ethernet2

Restart network:
systemctl restart network
```

## New Network Utilities
- Network configuration methods
    * ```nmtui```
    * ```nmcli```
    * ```nm-connection-editor```
    * GNOME Settings
- Getting started with NetworkMaanager
    * NetworkManager is a service that provides set of tools designed specifically to make it easier to manage the networking configuration on Linux systems and is the default network management service on RHEL8
    * It makes network management easier
    * It provides easy setup of connection to the user
    * NetworkManager offers management through different tools such as GUI, nmtui, and nmcli
- Network configuration methods
    * nmcli - short for network manager command line interface. This tool is useful when access to a graphical environment is not available and can also be used within scripts to make network configuration changes
        * ```nmcli con show```
    * **nmtui** - short for network manager text user interface. This tool can be run within any terminal window and allows changes to be made by making menu selections and entering data
    * nm-connection-editor - a full graphical management tool providing access to most of the NetworkManager configuration options. It can only be accessed through the desktop or console
    * GNOME Settings - The network screen of the GNOME desktop settings application allows basic network management tasks to be performed


## Downloading Files or Apps (wget)
- e.g. ```wget http://website.com/filename```
- Why?? Most of the servers in corporate environment do NOT have internet access


## curl and ping Commands
```
curl http://website.com/filename
curl -O http://website.com/filename    # Download file

ping www.google.com
ping -c4 www.google.com
```

## FTP - File Transfer Protocol
- The File Transfer Protocol is a standard network protocol used for the transfer of computer filebetween a client and server on a computer network. FTP is built on a client-server model architecture using seperate control and data connections between the client and the server.
- Protocol = set of rules used by computers to communicate
- Default FTP Port = **21**
    * SSH = 22
    * DNS = 53
- For the lecture:
    * Client = MyFirstLinuxVM
    * Server = LinuxCentOS7

#### Install and Configure FTP on the remote server
- Become root
```
rpm -qa | grep ftp
ping www.google.com
yum install vsftp
vi /etc/vsftp/vsftpd.conf    # make a copy first
```
- Find the following lines and make the changes as shown below:
```
## Disable anonymous login
anonymous_enable=NO

## Uncomment
ascii_upload_enable=YES
ascii_download_enable=YES

## Uncomment - Enter you Welcoming message (Optional)
ftpd_banner=Welcome to UNIXMEN FTP service

## Add at the end of the file
use_localtime=YES
```
```
systemctl start vsftp
systemctl enable vsftp
systemctl stop firewalld
systemctl disable firewalld
useradd jchen (if the user does not exist)
```
#### Install FTP client on the client server:
- Become root
```
yum install ftp
su - jchen
touch kruger
```
- Commands to transfer file to the FTP server
```
ftp 192.168.1.x    (ftp 172.20.10.9)

Enter username and password

bi           # switching to binary mode to transfer files
hash         # shows the progress
put kruger   # transfer file
bye
```

## SCP - Secure Copy Protocol
- The Secure Copy Protocol or "SCP" helps to transfer computer files securely from a local to a remote host. It is somewhat similar to the File Transfer Protocol "FTP", but it adds security and authentication
- Protocol = Set of rules used by ocmputers to communicate
- Default SCP Port = **22** (same as SSH)
#### SCP commands to transfer file to the remote server:
```
Login as yourself

touch xxxxxx
scp xxxxxx jchen@172.20.10.X:/home/jchen
Enter username and password
```

## rsync - Remote Synchronization
- **rsync** is a utility for efficiently transferring and synchronizing files within the same computer or to a remote computer by comparing the modification times and sizes of files
- rsync is a lot faster than ftp or scp
- This utility is mostly used to backup the files and directories from one server to another
- Default rsync Port = **22** (same as SSH)
- Basic syntax of rsync command:
    * ``` # rsync options source destination ```
- Install rsync in your Linux machine (check if it already exists)
    * \# rpm -qa | grep rsync
    * \# yum install rsync (On CentOS/Redhat based systems)
    * \# apt-get install rsync (On Ubuntu/Debian based systems)
- rsync file on a local machine
    * $ tar cvf backup.tar (tar the entire home directory (/home/jchen))
    * $ mkdir /tmp/backups
    * $ rsync -zvh backup.tar /tmp/backups/
- rsync a directory on a local machine
    * $ rsync -azvh /home/jchen /tmp/backups/
- rsync a file to a remote machine
    * $ mkdir /tmp/backups (create /tmp/backups dir on remote server)
    * $ rsync -avz backup.tar jchen@192.168.1.x:/tmp/backups
- rsync a file from a remote machine
    * $ touch serverfile
    * $ rsync -avzh jchen@192.168.1.x:/home/jchen/serverfile /tmp/backups


## System Updates and Repos
- **yum** (CentOS)
- **apt-get** (other Linux)
- The repositories and configuration files locations: ```/etc/yum.repos.d```
- yum does need an Internet access to go online and install a package to your system
- ```rpm``` (Red hat Package Manager)
    * rpm is used when you already have a package downloaded in your system and then you could install it locally by running the command rpm (usually use when the environment don't have internet access)
    * yum does all the thing for you, it downloads the package, install the packcage as well
```
rpm -qa | gerp package_name
rpm -e package_name    # remove package

yum install package_name
yun remove package_name
```

## System Upgrade / Patch Management
- Two type of upgrades
    * Major version = 5, 6, 7
    * Minor version = 7.3 to 7.4
    * Check version: **cat /etc/redhat-release**
- You cannot update Major version by yum command, nut you can update Minor version by ```yum update```
    * ```yum update -y``` (-y: yes)
- yum update vs. yum upgrade
    * upgrade -> delete previous packages
    * update -> preserve previous packages


## Create Local Repository from DVD
- Repository is something where all your packages are stored and then you could donwload and install (need internet access), when there is no internet access on the machine local repository in needed
- ``` createrepo```
```
su -
mount CentOS.iso
cd /
mkdir localrepo

cd /run/media/jchen/isoname
df -sh .    # check this file size

cp -rv /run/media/jchen/isoname/Packages/* /localrepo

cd /etc/yum.repos.d
rm -rf /etc/yum.repos.d/*

vi local.repo
[centos7]
name=centos7
baseurl=file:///localrepo/
enable=1
gpgcheck=0

createrepo /localrepo/

yum clear all

yum repolist all

# test
yum install tomcat
```

## Advance Package Management
- Installing packages
- Upgrading
- Deleting
- View package details information
- Identify source or location information
- Packages configuration files
```
wget package_url
rpm -hiv xxxxxxxx.rpm    # install package

rpm -qa | grep ksh
rpm -qi package_name     # package information

rpm -e package_name      # delete package

rpm -qc package_name     # package configuration file

which package_name       # find command path
rpm -qf command_path     # find out what package the command belongs to

````

## Rollback Updates and Patches
- Rollback a package or patch
    * **yum install \<package-name>**
    * **yum history undo \<id>**
- Rollback an update
    * Downgrading a system to mino version (ex. RHEL7.1, RHEL7.0) is not recommended as this might leave the system in undesired or unstable state
    * **yum update** = Update will preserve them
    * **yum upgrade** = Upgrade will delete obsolete packages
    * **yum history undo \<id>** 

## SSH and telnet
- Telnet = Un-secured connection between computers
- SSH = Secured
- Two type of packages for most of the services
    * Client package
    * Server package
```
ssh localhost

ps -ef | grep sshd    # check the service of sshd
# /usr/sbin/sshd this is the process that actually listening for all the incoming traffic, if this process is stop i will not be able to login to this machine anymore

systemctl stop sshd
ps -ef | grep sshd
# could not open any new sessions

systemctl start sshd
ps -ef | grep sshd
```

## DNS = Domain Name System
- DNS is a system that is used to translate a hostname to IP address
    * Hostname to IP -> A Record
    * IP to Hostname -> PTR Record
    * Hostname to Hostname -> CNAME Record
- Files:
    * /etc/named.conf
    * /var/named = all the zone files where you define all the hostname to IP, IP to hostname
- Service
    * **systemctl restart named**

### Download, Install and Configure DNS
1. Create a snapshot of your virtual machine
2. Setup:
    * Master DNS
    * Secondary or Slave DNS
    * Client
3. Domain Name = lab.local
4. IP address = My local IP address on enp0s3
5. Install DNS package
    * ```yum install bind bind-utils -y```
6. Configure DNS (Summary)
    * Modify **/etc/named.conf**
    * Create two zone files (forward.lab and reverse.lab)
    * Modify DNS file permissions and start the service
7. Revert back to snapshot

## Hostname or IP Lookup
- Commands used for DNS lookup
    * ```nslookup``` (older)
    * ```dig``` (more information)

## NTP = Network Time Portocol
- Purpose: Time synchronization
- File: /etc/ntp.conf
- Service: ```systemctl restart ntpd```
- Command: ```ntpq```
- NTP Port = **123**
```
rpm -qa | grep ntpd

vi /etc/ntp.conf
    server 8.8.8.8    # Google Server
    
systemctl start ntpd
systemctl enable ntpd
ps -ef | grep ntpd


ntpq
> peers
> quit

systemctl stop ntpd
ps -ef | grep ntpd

```

## chronyd (New Version of NTP)
- Purpose: Time synchronization
- Package name = chrony
- Configuration File: /etc/chrony.conf
- Log file = /var/log/chrony
- Service: ```systemctl start/restart chronyd```
- Program Command: ```chronyc```
- Don't run NTP and chronyd together (inactive NTP)
-  Port = ****
```
rpm -qa | grep chrony

vi /etc/chrony.conf

systemctl status chronyd

systemctl status ntpd
systemctl stop ntpd
systemctl disable ntpd

systemctl start chronyd
systemctl enable chronyd

chronyc
> sources

```

# New System Utility Command (timedatectl)
- The ```timedatectl``` command is a new utility for RHEL/CentOS7/8 based distributions, which comes as a part of the systemd system and service manager
- It is a replacement for old traditional ```date``` command
- The ```timedatectl``` command shows/change date, time, and timezone
- It synchronizes the time with NTP server as well
    * You can either use chronyd or ntpd and make the ntp setting in ```timedatectl``` as yes
    * Or you can use ```systemd-timesyncd``` daemon to syncheonize time which is a replacement for ntpd and chronyd
- Please note: Redhat/CentOS doesnot provide this daemon in its stanndard repo. You will have to download separately
- Commands:
```
# To check time status
timedatectl

# To view all available time zones
timedatectl list-timezones

# To set the time zone
timedatectl set-timezone "America/New_York"

# To set date
timedatectl set-time YYYY-MM-DD

# To set date and time
timedatectl set-time '2015-11-20 16:14:50'

# To start automatic time synchronization with a remote NTP
timedatectl set-ntp true
```

## SendMail
- Purpose: Send and receive emails
- Files: /etc/mail/sendmail.mc
        /etc/mail/sendmail.cf
        /etc/mail
- Service: ```systemctl restart sendmail```
- Command: ```mail -s "subject line" email@mydomain.com```
- Practice:
```
su -

# Download/Install Packages
rpm -qa sendmail
yum install sendmail
yum install sendmail-cf

cd /etc/mail

vi snedmail.mc

systemctl restart sendmail
systemctl status sendmail
ps -ef | grep sendmail

mail -s "mail setup" kuantachen1021@gmail.com
<type mail content>
Ctrl-d to end

```
- Mail relay server is a server that is set up in a company, an environment which will accpet your emails that are coming from any of the server and then deliver


## Web Server (Apache - HTTP)
- Purpose: Serve webpages
- Service or Package name = ```httpd```
- Files: /etc/http/conf/httpd.conf
        /var/www/html/index.html
- Service: ```systemctl restart httpd``` / ```systemctl enable httpd```
- Log Files: /var/log/httpd
- Practice:
```
rqm -qa | gerp httpd

cd /etc/httpd/conf

more httpd

cd /var/www/html/
touch index.html
vi index.html

# Open firewall
systemctl stop firewalld

# Start httpd server
systemctl start httpd
```

## Central Logger (rsyslog)
- Central Logger is a server that acta as a server and it receives all the logging information from every server/client.
- Purpose: Generate logs or collect logs from other servers
- Service or Package name = ````rsyslog```
- Configuration file: /etc/rsyslog.conf
- Service: ```systemctl restart rsyslog``` / ```systemctl enable rsyslog```
- Default port = **514**
- Practice:
```
rpm -qa | grep rsyslog
(yum install rsyslog)

vi /etc/rsyslog.conf

systemctl status rsyslog
```

## Securing Linux Machine (OS Hardening)
- User Account
    * Password information(e.g. expire date): ```chage -l username```
- Remove un-wanted packages
    * ```rpm -qa``` = list all of the packages
    * ```rpm -e``` = delete package (be aware of the dependecies)
- Stop un-used Services
    * ```systemctl -a``` = list all services
- Check on Listening Ports
    * ```netstat -tunlp``` = check ports that are opening now
- Secure SSH Configuration
    * cd /etc/ssh
    * more sshd_config
- Enable Firewall (iptables/firewalld)
    * ```firewall-config``` = pop GUI section
    * ```firewall-cmd``` = for command
    * ```iptables```
- Enable SELinux (Security Enhane Linux)
    * Is a security architecture integrated into 2.6X kernel, using the Linux Security Module (LSM)
    * It defines the access and transmission rights of every user, every application process and file system
    * ```sestatus```
    * cd /etc/sysconfig
- Change Listening Services Port Numbers
- Keep your OS up to date (security patching)


## OpenLDAP Installtion
- An open source implementation of lightweight directory access protocol.
- OpenLDAP Service
    * ```slapd```
- Start or Stop the service
    * ```systemctl start slapd```
    * ```systemctl enable slapd```
- Configuration Files
    * /etc/openldap/slapd.d
- Practice:
```
yum install *openldap*

systemctl start slapd
systemctl status slapd
ps -ef | grep slapd

cd /etc/openldap/slapd.d

```

## Tracing Network Traffic (traceroute)
- The ```traceroute``` command is used in Linux to map the journey that a packet of information undertakes from its source to its destination. One use for traceroute is to locate when data loss occurs throughtout a network, which could signify a node that's down
- Because each hop in the record reflects a new server or router between the originating PC and the intended target, reviewing the results of a traceroute scan also lets you identify slow points that may adversely affect your network traffic
- Example:
    * ```# traceroute www.google.com```
    * Gateway: ```netstat -rnv```

## How to Open an Image File
- Image file formats are standardized means of organizing and storing digital images. Image files are composed of digital data in one of these formats that can be rasterized for use on a computer display or printer. An image file format may store data in uncompressed, compressed, or vector formats
- Install a program that will allow us to open image file
    * ```yum install ImageMagick```
- Command syntax to open image file in GUI
    * ```display image-file```


## Configure and Secure SSH
- SSH = secure shell
    * Shell provides you with an interface to the Linux system. It takes in your commands and translate them to kernel to manage hardware
    * Open SSH is a package/software
    * Its service daemon is sshd
    * SSH port **#22**
- SSH itself is secure, meaning communication through SSH is always encrypted, but there should be some additional configuration can be done to make it more secure
- Following are the most common configuration an adminstrator should take to secure SSH:
- Configure Idle Timeout Interval
    * Avoid having an unattended SSH session, you can set an Idle timeout interval
        * Become root
        * Edit /etc/ssh/sshd_config file and add the following line:
        ```
        ClientAliveInterval 600
        ClientAliveCountMax 0
        ```
        * systemctl restart sshd
    * The idle timeout interval you are setting is in seconds (600sec = 10min). Once the interval has passed, the idle user will be automatically logged out
- Disable root login
    * Disabling root login should be one of the measures you should take when setting up the system for the first time. It disable any user to login to the system with root account
        * Become root
        * Edit your /etc/ssh/sshd_config file and replace PermitRootLogin yes to no
        ```PermitRootLogin no```
        * systemctl restart sshd
- Disable Empty Passwords
    * You need to prevent remote login from  accounts with empty passwords for added security
        * Become root
        * Edit your /etc/ssh/sshd_config file and remove # from the following line
        ```PermitEmptyPasswords no```
        * systemctl restart sshd
- Limit Users' SSH Access
    * To provide another layer of security, you should limit your SSH logins to only certain users who need remote access
        * Become root
        * Edit your /etc/ssh/sshd_config file and add
        ```AllowUsers user1 user2```
        * systemctl restart sshd
- Use a different port
    * By default SSH port runs on 22. Most hackers looking for any open SSH servers will look for port 22 and changing can make the system much more secure
        * Become root
        * Edit your /etc/ssh/sshd_config file and remove # from the following line and change the port number
        ```Port 22```
        * systemctl restart sshd
- SSH-Keys - Access Remote Server without Password

## SSH-Keys - Access Remote Server without Password
- Two reasons to access a remote machine
    * Repetitive logins
    * Automation through scripts
- Keys are generated at user level
    * jchen
    * root
- Practice:
```
Client = MyFirstLinuxVM

# Step1: Generate the key
ssh-keygen

# Step2: Copy the key to the server
ssh-copy-id root@192.168.1.x  (server)

# Step3: Login from client to server
ssh root@192.168.1.x
ssh -l root 192.168.1.x


# In server machine (LinuxCentOS7)

cd /root/.ssh
cat authorized_keys
```


## Linux Web-Based Adminstration (cockpit)
- Cockpit is a server adminstration tool sponsored by RedHat, focused on providing a modern-looking and user-friendly interface to manage and administer servers
- Cockpit is the easy-to-use, integrated, glanceable, and open web-based interface for your servers
- The application is available in most of the Linux Distributions such as, CentOS, Redhat, Ubuntu and Fedora
- It is installed in Redhat 8 by default and it is optional in version 7
- It can monitor system resources, add or remove accounts, monitor system usage, shut down the system and perform quite a few other tasks all through a very accessible web connection
### Install Configure and Manage Cockpit
1. Check for internet access
    ```ping -c2 www.google.com```
2. Install cockpit package as root
    ```yum/dnf install cockpit```
3. Start and enable the service
    ```systemctl start|enable cockpit```
4. Check the status of the service
    ```systemctl status cockpit```
5. Access the web-interface
    ```https://192.168.1.x:9090```
6. If cannot connect try to close firewall service
    ```systemctl stop firewalld```
    

## Firewall
### Introduction to Firewall
- What is Firewall?
    * A wall that prevents the spread of fire
    * When data move in and out of a server its packet information is tested against the firewall rules to seee if it should be allowed or not
    * In simple words, a firewall is like a watchman, a bouncer, or a shield that has a set of rules given and based on that rule they decide who can enter and leave
    * There are 2 type of firewalls in IT
        * Software = Runs on operating system
        * Hardware = A dedicated appliance with firewall software

### Firewall (iptables)
- There are 2 tools to manage firewall in most of the Linus distributions
    * ```iptables``` = For older Linux versions but still widely use
    * ```firewalld``` = For newer versions like 7 and up
- Before working with iptables make sure firewalld is not running and disable it
```
# To stop the service
service OR systemctl stop firewalld

# To prevent from starting at boot time
systemstl disable firewalld

# To prevent it from running by other programs
systemctl mask firewalld
```
- Now check if you have iptables-services package installed
```
rpm -qa | grep iptables-services
yum install iptables-services
```
- Start the service
```
systemctl start iptables
systemctl enable iptables
```
- To check the iptables
```
iptables -L
```
- To flush iptables
```
iptables -F
```

### Firewall (iptables - tables, chains and targets)
- The function of iptables tool is packet filtering
- The packet filtering mechanism is organized into three different kinds of structures: tables, chains and targets
    1. tables = table is something that allows you to process packets in specific ways. There are 4 different types of tables: filter, mangle, nat and raw
    2. chains = the chains are attached to tables, these chains allow you to inspect traffic at various points. There are 3 main chains used in iptables
        * INPUT = incoming traffic
        * FORWARD = going to a router, from one device to another
        * OUTPUT = outgoing traffic
            * chains allow you to filter traffic by adding rules to them
            * Rule = if traffic is coming from 192.168.1.35 then go defined target
    3. targets = target decides the fate of a packet, such as allowing or rejecting it. There are 3 different type of targets
        * ACCEPT = connection accepted 
        * REJECT = send reject response
        * DROP = drop connection without sending any response

### Firewall (firewalld)
- Firewall works the same way as iptables but of course it has it own commands
    * ```firewall-cmd```
- It has a few pre-defined service rules that are very easy to turn on and off
    * service such as: NFS, NTP, HTTPD etc.
- Firewalld also has the follwoing:
    * table
    * chains
    * rules
    * targets
- Make sure iptables is stopped, diable and mask
```
systemctl stop iptables
systemctl disable iptables
systemctl mask iptables
```
- Now check if firewalld package is installed
```
rpm -qa | grep firewalld
```
- Start firewall
```
systemctl unmask firewalld (if needed)
systemctl start/enable firewalld
```
- Check the rule of firewalld
```
firewall-cmd --list-all
```
- Get the listing of all services firewalld is aware of:
```
firewall-cmd --get-services
```
- To make firewalld re-read the configuration added
```
firewall-cmd --reload
```

### Firewall (firewalld - practical examples)
- The firewalld has multiple zone, to get a list of all zones
```
firewall-cmd --get-zones
```
- To get a list of active zones
```
firewall-cmd --get-active-zones
```
- To get firewall rules for public zone
```
firewall-cmd  --zone=public --list-all
OR
firewall-cmd --list-all
```
- All services are pre-defined by firewall. What if you want to add a 3rd party service
```
/usr/lib/firewalld/services/allservices.xml

# Simply cp any .xml file and change the service and port number

<?xml version="1.0" encoding="utf-8"?>
<service>
    <short>SSH</short>
    <description>To login</description>
    <port protocol="tcp port="22"/>
</service>
```
- To add a service (http)
```
firewall-cmd --add-service=http
```
- To remove a service
```
firewall-cmd --remove-service=http
```
- To reload the firewalld configuration
```
firewall-cmd --reload
```
- To add or remove a service permanently
```
firewall-cmd --add-service=http --permanent
firewall-cmd --remove-service=http --permanent
```
- To add a service that is not pre-defined by firewall
```
/usr/lib/firewalld/services/allservices.xml

# Simply cp any .xml file and change the service and port number (32)
cd /usr/lib/firewalld/services/
cp ssh.xml sap.xml

<?xml version="1.0" encoding="utf-8"?>
<service>
    <short>SAP</short>
    <description>This is a third-party application service</description>
    <port protocol="tcp port="32"/>
</service>

systemctl restart firewalld

firewall-cmd --get-services (to verify new service)
firewall-cmd --add-service=sap
```

- To add a port
```
firewall-cmd --add-port=1110/tcp
```
- To remove a port
```
firewall-cmd --remove-port=1110/tcp
```
- To reject incoming traffic from an IP address
```
firewall-cmd --add-rich-rule='rule fmaily="ipv4" source address-"192.168.0.25" reject'
```
- To block and unblock ICMP incoming traffic
```
firewall-cmd --add-icmp-block-inversion
firewall-cmd --remove-icmp-block-inversion
```
- To block outgoing traffic to a specific website/IP address
```
host -t a www.facebook.com = find IP address

firewall-cmd --direct --add-rule ipv4 filter OUTPUT 0 -d 31.13.71.36 -j DROP

firewall-cmd --reload
systemctl restart firewalld
```

## Tune System Performance
- What is tuned?
    * Tuned pronounced as tune-d
    * Tune is for system tunning and d is for daemon
    * It is systemd service that is used to tune Linux system performance
    * It is installed in CentOS/Redhat version 7 and 8 default
    * tuned package name is tuned
    * The tuned service comes with pre-defined profiles and settings
    * Based on selected profile the tuned service automatically adjust system to get the best performance. e.g. tuned will adjust networking if you are downloading a large file or it will adjust IO settings if it detects high storage read/write
    * The tuned daemon applies system settings when the service starts or upon selection of a new tuning profile


| Tuned Profile | Purpose |
| ------ | ------ | 
| balanced | deal for systems that require a compromise between power saving and performance |
| desktop     | Text     |
| Throughput-performance     | Text     |
| Latency-performane     | Text     |
| network-latency     | Text     |
| Network-throughput     | Text     |
| powersave     | Text     |
| oracle     | Text     |
| virtual-guest     | Text     |
| virtual-host     | Tunes the system     |

- Check if tuned package has been installed
```
rpm -qa | grep tuned
```
- Installed tuned package if NOT installed already
```

```
- Check tuned service status
```
systemctl status|enable|start tuned
```
- Command to change setting for tuned daemon
```
tuned-adm
```
- To check which profile is active
```
tuned-adm active
```
- To list available profiles
```
tuned-adm list
```
- To change to desired profile
```
tuned-adm profile profile-name
```
- Check for tuned recommendation
```
tuned-adm recommend
```
- Turn off tuned setting daemon
```
tuned-adm off
```
- Change profile through web console
    * Login to https://your_ip:9090
    * Overview -> Configuration -> Performance profile
- Another way of keeping your system fine-tuned is by prioritizing processes through ```nice``` and ```renice``` command
- If a server has 1 CPU then it can execute 1 computation/process at a time as they come in (first come first served) while other processes must wait
- With nice and renice commands we can make the system to give preference to certain processes than others
- This priority can be set at 40 different levels
- The nice level tange from -20 (highest priority) to 19 (lowest priority) and by default, processes inherit their nice level from their parent, which is usually 0
- To check process priority
    * Nice value is a user-space and priority PR is the process's actual priority that use by Linux Kernel. In Linux system priorities aer 0 to 139 in which 0 to 99 for real time and 100 to 139 for users
```
top
```
- Process priority can be viewed through ps command as well with the right options
```
ps axo pid,comm,nice,cls --sort=nice
```
- To set the process priority
```
nice -n # proces-name
e.g. nice -n -15 top
```
- To change the process priority
```
renice -n # process-id
e.g. renice -n 12 PID
```


## Run Containers
- What is a Container?
    * The term container and the concept can from the shipping container
        * These containers are shipped from city to city and country to country
        * No matter which part of the world you to go, you will find these containers with the exact same measurements
        * Because around the world all docks, trucks, ships and wharehouses are built to easily transport and store them
    * The container technology allowed developers or programmer to test and build applications on any computer just by putting it in a container (bundle in with the software code, libraries and configuration files) and then run on another computer regardless of its architectuer
    * You can mvoe the application anywhere without moving its OS just like moving the actual physical container anywhere that would fit on any dockyard, truck, ship or warehouse
    * An OS can run single or multiple containers at the same time
- Container technology is mostly used by developers or programmers who write codes to build applications. As a system administrator your job is to install, configure and manage them
- What are the Container Software?
#### Docker
- Docker is the software used to create and manage containers
- Just like any other package, docker can be installed on your Linux system and its service or daemon can be controlled through native Linux service management tool
#### Podman
- Podman is an alternative to docker
- Docker is not supported in RHEL8
- It is daemon less, open source, Linux-native tool designed to develop, manage, and run containers

### Getting Familiar with Redhat Container Technology
- RedHat provides a set of command-line tools that can operate without a container engine, these include:
    * podman
    * buildah
    * skopeo
    * runc
    * crun

### Getting Familiar with podman Container Technology
- images = containers can be created through images and containers can be converted to images
- pods = group of containers deployed together on the host. In the podman logo there are 3 seals grouped together as a pod

### Building, Running and Mananging Containers
- To install podman
```
yum install podman -y
yum install docker -y
```
- Creating alias to docker
```
alias docker=podman
```
- Check podman version
```
podman -v
```
- Getting help
```
podman --help OR man podman
```
- Check podman environment and registry/repository information
```
podman info

# If you aer trying to load a container image, then it will look at the local machine and then go through each registry by the order listed
```
- To search a specific image in repository
```
podman search httpd
```
- To list any previously download lodman images
```
podman images
```
- To download available images
```
podman pull docker.io/library/httpd
podman images (check download image status)
```
- To list podman running containers
```
podman ps
```
- To run a download httpd container
```
podman run -dt -p 8080:80/tcp docker.io/library/httpd

# d=detach, t=get the tty shell, p=port

podman ps
```
- To view podman logs
```
podman logs -l
```
- To stop a running container
```
podman stop con-name
```
- To run a multiple containers of httpd by changing the port #
```
podman run -dt -p 8081:80/tcp docker.io/library/httpd

podman ps
```
- To stop and start a previously running container
```
podman stop|start con-name
```
- To create a new container from the downloaded image
```
podman create --name httpd-con docker.io/library/httpd
```
- To start newly create container
```
podman start httpd
```
- Manage containers through systemd
```
# First you have to generate a unit file
podman generate systemd --new --files --name

# Copy it systemd directory
cp /root/container-httpd.service /etc/systemd/system

# Enable the service
systemctl enable container-httpd.service

# Start the service
systemctl start container-httpd.service
```


## Kickstart (Automate Linux Installation)
- Kickstart is a method to automate the Linux installation without the need for any intervention from the user
- With the help of kickstart you can automate questions that are asked during the installation
- To use a Kickstart, you must:
    1. Choose a Kickstart server and create/edit a Kickstart file
    2. Make the Kickstart file available on a network location
    3. Make the installation source available
    4. Make boot media available for client which will be used to begin the installation
    5. Start the Kickstart installation

- CentOS/Redhat 7
    * Kickstart program can be downloaded which allows you to define parameters through the GUI ``` yum install system-config-kickstart ```
    * Or you can use the installation kickstart file which was created during the first installation (anaconda-ks.cfg)
- CentOS/Redhat 8
    * There is no GUI available to edit the file
- Why changed?
    * Most systems are virtual and templates can be used
    * Automation software are in used such as Anisble

- Step by step procedure for Kickstart
```
1. Identify the server

2. Take a snapshot of the server

3. Install kickstart configurator (for version 7)
    yum install system-config-kickstart

4. Start the kickstart file configurator and define parameter OR use the /root/anaconda-ks.cfg
    system-config-kickstart
    We will use anaconda installation kickstart file and change the hostname only

5. Make sure httpd package is installed, if not then install the package and start the httpd service
    rpm -qa | grep http
    yum/dnf install httpd
    systemctl start httpd
    systemctl status httpd
    systemctl enable httpd

6. Copy kickstart file to httpd directory and change the permission
    cp /root/anaconda-ks.cfg /var/www/html
    chmod a+r /var/www/html/anaconda-ks.cfg
    systemctl stop|disable firewalld
    # Check file through browser on another PC http://192.168.1.x/anaconda-ks.cfg
    
7. Create a new VM and attach the CentOS iso image

8. Change the network adapter to Bridged adapter

9. Hit Esc

10. boot: linux ks=http://192.168.1.x/anaconda-ks.cfg
# For NFS => boot: linux inst.ks=nfs:192.168.1.x:/rhel8

11. Wait and enjoyed the automated installation
```
- Kickstar for clients with static IP
```
boot: linux ks=http://server.example.com/ks.cfg ksdevice=eth0 IP:192.168.1.50 netmsk=255.255.255.0 gateway=192.168.1.1

Where:
    ksdevice = is the network adapter of the client
    IP = IP you are assigning to the client
    netmsk = Subnet mask for the client
    gateway = Gateway IP address for the client
```

## DHCP
- DHCP = Dynamic Host Configuration Protocol
- In order to communicate over the network, a computer needs to have an IP address
- DHCP server is responsible to automatically assign IP addresses to servers, laptops, desktops, and other devices on the network
- Right now in our home how IPs are assigned to our devices?
    * The router or gateway given to you by your ISP provider
- How IPs are assigned in coporate world?
    * Dedicated routers run DHCP service to assign IPs on the network
