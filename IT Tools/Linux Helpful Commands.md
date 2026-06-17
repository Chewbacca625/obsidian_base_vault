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
		
