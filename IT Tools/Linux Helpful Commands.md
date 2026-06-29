- Filesystem Hierarchy
	- / - root top level directory
		- /bin - user binary exe
		- /sbin - maintenance binaries 
		- /boot - kernel loading & bootloader
		- /dev - interface for hardware nodes
		- /etc - system config holds static config files for system and apps (no user data here)
		- /home - user profiles reside, docs, etc
		- /root - isolated space for admin files
		- /lib - shared libraries critical for operation of binaries in /bin and /sbin 
		- /opt - dedicated space for third party software installed outside of package manager
		- /media - automatic mount point for removal media like USB
		- /mnt - reserved for manual, temp mounting of file system by admin
		- /tmp - temp files for system and user file (cleared by OS)
		- /usr - user facing binaries and docs, static & shareable, /usr/local for custom installs
		- /var - variable files , system logs, spools, dbs, for dynamic growth
Command structure
cmd - options - argument
man - man pages
- man man will allow you to select specific sections of man pages
apropos - search the manual page names and descriptions
- if you spin up the machine for the first time you need to sync the man page DB
	- run: sudo mandb
- commands exist in section 1-8, we can use -s flag to select specific sections
tab tab - helps you find a list of options for a command

ls -lah - l -> list, a -> all, h -> human readable size
pwd - print working directory
cd .. - # go to previous directory
cd ~ # go to home directory
	../jane/reciept.pdf
	 /home/jane/reciept.pdf - same as above
cp # copy file/dir
mv # move file or dir or rename
mkdir -p - make dir (-p) build all child directories too
rmdir - remove empty dir
stat # info about a file
	hard links - let say two user account want to share one file we can link them with (pointed to inode): ln
		- only hardlink to files 
		- only hardlink files on same filesystem
	soft links - shortcut for a file or directory or on a different filesystem
		cmd: ln -s 
		![[Screenshot 2026-06-17 at 10.40.02 AM.png]]
		- the l in ls -l show that this is a soft link and to access the link we can use readlink
chgrp - change group on file
groups - list user groups
chown - change owner only root can do

first character shows the file type 
![[Screenshot 2026-06-18 at 7.26.23 AM.png|478]]
-**rwx(Owner)** *rwx(Group)* rwx(other)
- owner - permissions 
- group - permissions
- other - permissions
![[Screenshot 2026-06-18 at 7.29.43 AM.png|224]]
		
chmod - change mode user perms on a file (+(add), -(subtract), =(set exactly))(rwx)
![[Screenshot 2026-06-18 at 7.33.21 AM.png|308]]
- example: chmod u+rw,g=r,o= xyz.txt
![[Screenshot 2026-06-18 at 7.39.28 AM.png|385]]
- octal permissions
![[Screenshot 2026-06-18 at 7.40.06 AM.png]]
- handled in binary 0640 = r(1)w(1)(0)=6, r(1)-(0)-(0) = 4, -(0)-(0)-(0) = 0 == 640

SUID, SGID, Sticky Bit, umask
- umask - defines default security posture and subtracts bits from default OS setting to define initial perms for new files and dirs
- SUID special perm that allows users to run exe with perms of exe's owner
	![[Screenshot 2026-06-18 at 7.51.10 AM.png|417]]
- SGID similar to SID but applies to both exe and directories, it allows users to temporarily inherit the group ownership when running exe or dir
- Sticky bit can be set on a directory and prevents files from being deleted from in that dir, only file owner, dir owner, or root
![[Screenshot 2026-06-18 at 7.57.21 AM.png|498]]
- chmod 4xxx is the SUID
- **S** - suid is enabled but cannot execute
- **s** - suid is enabled and execute is enabled
![[Screenshot 2026-06-18 at 8.00.10 AM.png|430]]
- chmod 2xxx is the GID
- **S** - suid is enabled but cannot execute
- **s** - suid is enabled and execute is enabled
- chmod 6xxx sets GID and SUID
![[Screenshot 2026-06-18 at 8.02.06 AM.png|432]]
- to find files with SUID or SGID

![[Screenshot 2026-06-18 at 8.04.29 AM.png|546]]
- chmod 1xxx - sticky bit
- **T** - sticky is enabled but cannot execute
- **t** - sticky is enabled and execute is enabled

- cat to print file contents | tac to reverse
- tail to see the last 10 lines of a file (-n to customize how many lines) | head to see the beginning of the file
- sed stream editor to edit files 
	- example sed 's/canda/canada/g' xyz.txt -i
		- s - subsitute cmd
		- g - global search the whole file
		- -i edit in place
		- you can use / or | as delimiter
		- need to \\$ because it will be treated as end of line
		- -E for extended regex
		- you can use d to delete lines{}
- cut extracts specific sections based on delimiter (-d) and field (-f)
- uniq removed repeated lines
- sort sorts text in a file
- diff compares file differences
	![[Screenshot 2026-06-23 at 2.33.32 PM.png]]
	![[Screenshot 2026-06-23 at 2.34.21 PM.png]]
	- -c flag provides full context of the file differences
	- sdiff side-by-side comparison 
	- less | more are pagers to view long txt files or cmd outputs - read only 
	- Vim
		- dd delete line
		- p paste line
		- y - yank/copy line
	- Grep
		- -i case insensitive
		- -r recursively search through directory
		- --color for when you use sudo and need color
		- -v dont contain search pattern
		- -w word option to search for specific word pattern
		- -o for only matching to return only specified pattern
		- use egrep to avoid having to use \ for regex
	- Regex Patterns
		- ^ - beginning of string ex: '^PASS' - PASSWORD?
		- $ - end of string 'mail$' - mail files
		- . - match any one character 'c.t' - return cat or cut
		- + - match the previous element 1 or more times
		- {min,max} - previous element can exist "this many" times
		- ? - make previous element options 1 or not at all
		- \[] - ranges or set \[a-z] (any lowercase letter a-z will be matched) \[0-9] (any one digit 0-9) ex: \[abz954] - only abz954 characters
		- \() - subexpression apply regex to everything in the parenthesis 
		- \[\^] - negate ranges or sets 
		- \ is an escape character we can use if we need to escape a regex
		- \* can be used to match the previous element 0 or more time ex: let* = le, let, lett,...
		- | or operator ex: egrep -wr 'enabled? | disabled?' /etc/
		- '\bword\b' - word boundary search
- Globing Patterns
	- * match zero or more chars \*.txt
	- ? match exactly one char image?.txt
- Date Referencing
	- Hardlink - direct pointer to Inode
		- additional entry for the same data block
		- limited to same file system
		- persistent even if original entry is removed 
	- Symbolic link - path based shortcut
		- created via ln -s
		- can span diff partitions & mount points
		- broken if original file is deleted
- Redirection
	- < - feed file contents as input
	- > - save to output file (overwrite)
	- >> - add to existing file (append)
- locate - queries updatedb, not that it can be outdated and requires root to update
- which - scans PATH env var - returns first abs pathname of an exe
- whereis - find binaries, source, manuals - searches standard system directories 
- apt (fetch resolve dependencies) high-level -> dpkg (unpacks) low-level
- dnf/yum high-level logic -> RPM (handles physical install)
- Linux IAM
	- /etc/passwd (readable)
		- Username & UID
		- Group ID (GID)
		- Home Dir
		- Default Shell
	- /etc/shadow (privileged)
		- password hashes
		- account expiry
		- aging info
		- secret data
	- /etc/group
		- Group Name -> GID
		- Secondary members
		- Group Definitions
		- Membership Maps
	- useradd - create new user
	- userdel - delete user
	- usermod - modify user
	- groupadd - create new OU's / groups
	- usermod - to assign groups
- /etc/shadow
	- passwd - initial password setup and manual modification of auth tokens
	- chage - org standards password exp, min days between changes, exp warnings
- avoid editing sudoers file in standard text editor
	- use visudo - to validate syntax before saving
- Boot process
	- firmware - bios / UEFI
	- Boot Loader - GRUB2
	- Kernel  - Init RAM Disk | mount root FS
	- Init system - systemd - user space start
- systemctl - manage system services
	- status - check status of service
	- disable - disable system start
	- start/stop/restart
	- isolate - for config target unit
	- Target units - group services into operational modes
		- multi-user.target
			- no gui
			- background processes
			- networking enabled
		- graphical.target
			- multi-user dependencies
			- display manager
			- desktop env
- journald: source of truth for troubleshooting kernel, boot process, user space
	- journalctl: filter processes very granularly 
	- systemd-analyze: timing stats
- Process management
	- PID: process ID
	- ps aux - static snapshot of every process executing (terminal status, start time, cpu & ram)
	- top/htop - dashboard to sort resource usage and kill runaway processes
	- kill cmd kills running processes softly or forced 
		- kill -sigterm PID - soft
		- kill -9 PID - force kill
		- sighub - refresh or reload config
	- foreground vs background execution
		- cmd - waits for process to complete
		- cmd & - run shell stays interactive
		- ctrl+ z - suspend 
		- bg - move to bg
		- fg - move to fg
	- top - 1,5,15 min load averages
		- excessive swap to disk degrades system performance to unresponsiveness
- Storage and File Systems
	- lsblk - list blocks
	- df -h : human readable summary of occupied and available blocks
	- fdisk - modify disk partion tables
	- EXT4 : general purpose
	- XFS : large strorage focus, enterprise scale
	- Btrfs : modern approach, instant snapshots, copy on write, volume mgmt
	- mount cmd is ephemeral unless you update the /etc/fstab for automatic attachment of disks
	- LVM - pools resources (physical volumes) -> volume group -> logical volume control size of volumes
	- df cmd to identify disk usage to ensure tmp,logs,etc. dont overfill
	- isolate /var and /home to prevent runaway processes from stalling OS
- Logging & Observability
	- /var/log - hub for all sys artifacts (auth.log(logins), syslog(general events), dmesg(hardware failures))
	- rsyslog - shipping to central logging server
- System Automation
	- Cron dameon - primary tool for recurring tasks
	- each user can have their own crontab file
	- cron tab syntax (minutehourday/monweekday) /abs/path/to/cmd
	- ex: 02*** /abs/path/to/file
		- wildcard runs for every possible value
		- this will repeat at 2 am every day
	- cron runs as owner of file
	- shorthands exist @daily