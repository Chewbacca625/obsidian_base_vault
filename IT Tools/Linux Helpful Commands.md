man - man pages
- man man will allow you to select specific sections of man pages
apropos - search the manual page names and descriptions
- if you spin up the machine for the first time you need to sync the man page DB
	- run: sudo mandb
- commands exist in section 1-8, we can use -s flag to select specific sections
tab tab - helps you find a list of options for a command

ls -lah - l -> list, a -> all, h -> human readable size
pwd - print working directory
cd - # go to previous directory
cd # go to home directory
	../jane/reciept.pdf
	 /home/jane/reciept.pdf - same as above
cp # copy file/dir
mv # move file or dir or rename
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

SUID, SGID, Sticky Bit
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
![[Screenshot 2026-06-18 at 8.03.35 AM.png|602]]
- chmod 1xxx - sticky bit