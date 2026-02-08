# Linux Shell Basics

A shell is a user interface to the OS, and it allows us to run programs to perform tasks such as manipulating files, running applications, etc. A command-line shell is a window where we have to type all the commands, rather than for example clicking on buttons.

This note will be about the bash shell for the Linux OS. 

Linux organizes information using a filesystem. Special files which contain other files are known as directories. The filesystem is best viewed as a tree rooted at a root directory (denoted by /). Then, we can uniquely specify the location of any file by the path we need to take. For example, /home/notes/linux_shell.md Note that / serves as a path separator. If the path ends with /, eg /home/notes/ then we know notes is a directory.

Here are some basic commands for navigation in the shell:

- ls: List all non-hidden files in the directory where the command is executed. To include hidden files, add -a.
- man (x): Gives the manual page for command x. For example, man ls.
- pwd: Prints the name of the current directory
- cd (x): Change directory to x. Note that if say we are in /home/notes/ and we want to change to /home/notes/test we can do cd test rather than cd /home/notes/test. This is known as using the relative path
- mkdir (x): Creates directory x

Here are some special directories to know:

- Dot (.) : Refers to the current directory
- DotDot (..) : Refers to the parent directory of the current directory
- Tilde (~) : Refers to the home directory

Here are some more commands concerning file manipulation:

- cat (x, y, ...): Prints out the concatenation of all the files given as arguments. If given a file, just prints out the content of that file.
- touch (x): Create an empty file x
- cp (x) (y): Copies file x to destination y
- mv (x) (y): Moves file x to destination y
- rm (x): Removes file x. It also removes directories by recursively deleting everything inside. Be careful with this because once removed, it is gone.
- zip/unzip (name) (x, y, ...): Creates a single file called name containing the other files given as arguments.
- find: search for files matching a certain criteria

One thing we might want to do is to connect to a remote host by using an SSH connection. We do using the ssh command. Once we enter the remote server, any commands we execute on the shell will be executed on that machine. One thing we can do is remote copying using scp, to copy files from the remote server to out local machine (or vice versa). 

Let's say we want to edit a file. We need to use a text editor for that. One text editor which comes with the command-line shell is Vim. To use it, just do vim filename. There are two modes: Command and Insert. You can only edit the text in Insert mode, which you get by typing i. To save and exit, type Escape, then :wq and Enter.
