###Looking at files:
- head -n 1 - takes first line
- tail -f - stream end of file

###Control system/users/network:
- w - logged users<br />
- top (or htop needs to be installed) - list all processes
- sudo netstat -tupln - open network ports tcp/udp

###Pipes/redirects:
- stdin 0
- stdout 1
- stderr 2
- tee - for two outputs.. Command line and file e.g.: ls -al | tee text.txt - will save output to file and also print to command line

###Finding:
- find - use file system
- locate - used database
- which - find command binaries
- xargs - execute command on every each file
	- find / -name *.conf | xargs wc -l
- xargs -I{} -n 1 mv {} test.txt - rename all files to test.txt.. -I{} is variable with piped data

###Filter/Sort/Grep/Sed:
- cut -d: -f2 - pick whatever is after “:"
- sort -bf
- uniq - unique in file
- wc - word count
- egrep test ./* | uniq | cut -d: -f1 - use always egrep it has better regex support
- sed -e ’s/abc/123/‘ - replace abc to 123 if matching is successful (s/{find}/{replace}/)
- rev - reverse line
- tr - translate, replace, delete chars from input

####Example: cat /etc/passwd | cut -d: -f1 | tr '\n' ‘ ‘ - this will put all users in system into one line

###Install software:
####From source code:
		- ./configure
		- make
		- sudo make install
		- ldconfig - sometimes fix configuration
		- apt-get install build-essential - sometimes when getting configure or install errors this could fix it

####Packpage manager (APT - Debian):
	- apt-get
	- apt-cache<br />
	- add-apt-repository ppa:nginx/stable - for adding new repo to package manager
	- cat /ect/apt/sources.plist - here are listed all repos
  
####Tasksel:
- Ubuntu preinstalled packpages eg. web, mail, ssh server

###Permission:
- 4 - read
- 2 - write
- 1 - execute
- chmod -R - for all nested files/dirs
- chown - change ownership
- chgrp - change group
- umask - for changing default permissions (/etc/login.defs - UMASK for setting up default permission)
- whoami - current user
- id - current users and all groups he belongs to

###Users:
- sudo -i - switch to root
- cat /etc/passwd - list of all users in system (! instead of salt hash means that user can’t log in)
- cat /etc/shadow - for users passwords info
- cat /etc/groups - groups in system
- ls -al /etc/skel - default home directory content for new user
- groups - show me all groups i am in
- useradd -m -d /home/janek -s /bin/bash janek - create
- userdel janek - delete
- usermod - modification
- passwd janek - change pass
- newusers - handy tool for creating bunch of users
- skill -KILL -u janek - logout user
- ps -o pid -u janek - show all processes run by user
- pkill -u janek - kill all processes for user

###Proceses: signals/state/nicesness:
- man 7 signal - manual entry..
- kill, killall, pkill -u karel, etc…
- nice, renice -5 {pid} - change process niceness
- ls /proc - this is mounted by kernel and show all running processes
- man strace, strace -e open ls - for debugging process.. Here it is “ls”
- ctrl + z - stop running process
- jobs - view all stopped processes in current process
- fg %1 - put job from jobs list (1 is id from job list) run in foreground
- bg %1 - put job from jobs list (1 is id from job list) run in background

###File system:
- file - test file content
- man hier
  - /boot - actual Linux
	- /proc - mounted folder by kernel to show info about running process, hardware etc..
	- /etc - configurations for processes<br />
	- /sbin - basic linux programmes binaries (mostly by root)
	- /bin - normal binaries meant all system programs, often just simlinked to real destination
	- /tmp - temporary data generated and used by processes. Every process clean it after termination
	- /lib - shared librares
	- /lib64 - 64 bit libraries
	- /dev - devices
	- /mnt (/media - on Ubuntu) - for mounting files
	- /opt - optional software not installed by package manager
	- /usr - less important software
####File types:
- b - block, disk
- d - directory
- l - link
- c - one character devices (mouse, keyboard etc..)

###Crons:
- crontab -l - list
- crontab -e - edit
- crontab -e -u karel - as root
- /var/spool/cron/crontab - user specific crontabs
- /etc/cron.d - programme specific cron tabs
- /etc/cronatb - system wide crontab
- /etc/cron.allow - allow crons to certain users
- at - for just one time run job
- atq - see due of scripts
- atrm - remove script from que

###Network:
- cat /etc/protocols - available protocols on system
- cat /etc/network/interfaces - interfaces config.. set up of static address, broadcast, gateway etc..
	- auto eth0 = Auto Negotiate Speed for Ethernet Card 0
	- iface eth0 inet static /dhcp - Ethernet Card 0 Either Static or DHCP Address. If DHCP Don't Go Further.
	- address 192.168.1.100 - Static IP Address
	- netmask 255.255.255.0 - Subnet mask
	- network 192.168.1.0 - Network (Generally Your IP Address Siply with a 0 in the Last Octet))
	- broadcast 192.168.1.255 - Broadcast Address (The Last Address in Your Subnet. Generally Your IP Address with a 255 in the Last Octet.)
	- gateway 192.168.1.1 - Default Gateway. Generally Your ISP Modem or Router
-cat /etc/resolve.conf - configure DNS
- /etc/hosts —- names to ip addresses
- /etc/networks —- network names to ip addresses
- /etc/services —- tcp/udp service names to port numbers
- iwconfig - for wireless interfaces
- ifconfig - interface config
- ip route - get gateway
- dhclient - renew automatically IP address
- service networking restart - after every changed configuration
- hostname - box hostname
- dig - resolve domain name
- netstat -tupln - all open connection tcp,udp and their process ids
- traceroute - find the whole route to server
- ping - echo test
- host - to to all dns lookup
- nmap (need to be installed)
- macchanger - for change mac address
- ab - form http apps performance load tests
- tcpdump - useful for debugging network
- iptables - firewall
- ufw - Ubuntu easier firewall

###Archiving/Compression:
- tar -z/j (gzip/bzip2), c/x (create, extract), v (verbose), f {filename} {folderToArchive}
- gzip, gunzip - gzip algorithm
- bzip2, bunzip2 - bzip algorithm
- zip, unzip - for zip files.. Mainly use on MS Dos systems

###Shell:
- cat /etc/shells
- chsh /etc/sh - permanently change login shell
- echo $SHELL
- ps -p $$

####BASH shell config:
  - .bashrc - aliases, prompt config
	- .bash_profile - PATH env variable

###Kernel:
modules for kernel - drivers for pieces of hardware

####Kernel Modules:
- /lib/modules
- lsmod - list kernel modules
- rmmod - remove kernel module
- modprobe - install/load module with all dependencies

###Storage devices:
- fdisk -l - list all storage devices
- mkfs - make file system..
- fsck - file system check and repair.. work just on unmated devices
- df -h - display disk usage
- du -sh ~ - disk usage of folder
- parted /dev/sda1 “printed” - check file system type
- mount - to mount disk
- umount - to un mount disk
- /etc/fstab - set up mounting automatically (man fstab)

###Logs:
####/var/log/
- dmesg - boot logs, or just dimes command

###Init:
- dmesg- see boot logs
- init - command
- systemctl (service - on older systems) - for services defined in init configs
- start/stop - for current state
- enable/disable - during boot process
- status - speak for itself
- list-units - get overview
- journalctl -u serviceName - for viewing logs of systemd services

####Run levels
- runlevel - to see current run level
- Ubuntu sys: /etc/init/rc-sysinit.conf
- RedHat like sys: /etc/inittab 
