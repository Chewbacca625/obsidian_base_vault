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