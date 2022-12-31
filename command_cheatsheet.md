# Table of Contents
- [Shell Commands](#shell-commands)
	- [cp command examples](#cp-command-examples)
	- [mv command examples](#mv-command-examples)
	- [rm command examples](#rm-command-examples)
- [Filters](#filters)
- [File Permissions](#file-permissions)
	- [chmod comman examples](#chmod-examples)
- [Process Control](#process-control)
	- [kill signals](#kill-signals)
- [Wilcard Characters](#wildcard-characters)
	- [wilcard command examples](#wildcard-command-examples)
	- [Posix Character Class](#posix-character-class)

</br>

# Shell Commands
|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|$$|Get the current pid of the program|`$$`|echo $$|
|alias|Create a new command|`alias <new-command>=<value>`|alias l='ls -l'|
|cd|Change the working directory|`cd <directory>`|cd ~|
|cp|Copy files and directories|`cp <file1> <file2>`|cp file1 file1.1|
|date|Prints the date and time to standard output. Use a `+` to invoke formatting|`date +<formatting>`|date +%j|
|df|Provides drive  summary info including space used, space available, mount location|`df`|df
|file|Will display the type of file we pass in as an argument|`file <file>`|file commands.txt|
|find|Search for files or directories based on the given character expression|`find <in_directory> <expression>`|find . -type f|
|gzip|Used to handle single file compression|`gzip <file>`|gzip filename|
|help|Provide info on **built-in** shell commands|`help <command>`|help cd|
|--help|Provide info on **executable program**|`<command> --help`|mkdir --help|
|id|Tells us who the current user is|`id`|id|
|less|View text files|`less <file>`|less commands.md|
|let|Allows for arithmetic expressions|`let <expression>`|a=255; let "a++"|
|ln|Create symbolic links to files|`ln <target-file> <link-name>`|ln commands.md summary|
|lpr|Print files|`lpr <file>`|lpr filetoprint|
|ls|List content in the current working directory|`ls <options> <directories>`|ls -la /bin /etc|
|man|Formal documentation for commands (uses *less* command to view doc)|`man <command>`|man ls|
|mkdir|Create directories|`mkdir <directory>`|mkdir ./commands|
|mv|Move or rename files and directories|`mv <file1> <file2>`|mv ./file1 ./example/file2|
|pr|Convert text files for printing|`pr <file> > <printfile>`|pr file1 > filetoprint|
|printenv|Print out the environment variables|`printenv`|printenv|
|pwd|Print the name of the **current working directory**|`pwd`|pwd|
|RANDOM|Generate a random number|`$RANDOM`|echo $RANDOM|
|read|Read keyboard input|`read <variable-to-assign-input>`|read input|
|rm|Remove files or directories|`rm <file>`|rm -r ./directory|
|source|Executes a script|`source <script_file>`|source ~/.bashrc|
|tar|Used to archive multiple files|`tar <file/directory>`|tar directory|
|trap|Execute a command when our program receives a signal|`trap <commands to execute> <list of signals>`|trap "rm file; exit" SIGINT SIGTERM| 
|type|Check command type|`type <command>`|type ls|
|uptime|Provides system info on running time, last re-boot time, number of users, and system loads|`uptime`|uptime|
|wc|Count the number of files or directories|`wc`|wc|
|which|Provide the location of the given **executable** program|`which <executable>`|which ls|

### *cp command examples*
```bash
# copy contents of file1 into file2 (created or overwritten) -- /file1 /file2
cp file1 file2

# copy contents of file1 into dir1 as another file named file1 -- /dir1/file1
cp file1 dir1

# copy contents of dir1 into dir2 (created if does not exist) -- /dir2/dir1
cp -R dir1 dir2

# copy all files ending in '.txt' to directory 'text_files'
cp *.txt text_files
```

### *mv command examples*
```bash
# if file2 does not exist, file1 renamed to file2, else if file2 exists replace contents with file1
mv file1 file2

# moves file1 and file2 to dir1 if it exists (error otherwise)
mv file1 file2 dir1

# if dir2 does not exist, rename dir1 to dir2, else if dir2 exists move dir1 into dir2
mv dir1 dir2

# move 'dir1' and all files ending in '.bak' into dir2
mv dir1 ../*.bak dir2
```

### *rm command examples*
```bash
# delete file1 and file2
rm file1 file2

# delete dir1 and dir2 including all contents
rm -r dir1 dir2

# delete all files that end in a '~' character
rm *~
```

### *compression examples*
```bash
# compression using gzip - will not save a copy of original file
gzip filename

# gzip decompression using -d
gzip -d filename.gz

# tar compression - # -c to create a new archive, -v for verbose output, -f specify arhive name
# maintains the original file/directory
tar -cvf <archive-name> <filename/directory>

# compressing a tar archive will output a file with extension .tar.gz
gzip directory.tar

# shorthand for compressing a tar archive - # -z denote a zipped archive, .tgz is an abbr for .tar.gz
tar -czvf directory.tgz <file/directory>

# unpack a .tgz file - # -x to expand the .tgz file
tar -xvf directory.tgz
```

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Filters
|Filter|Description|
|:-:|:-|
|awk|Programming language for constructing filters|
|du|Summarize disk usage of files or directories|
|false|Will exit with a failure status|`false`|false|
|fmt|Reads text from standard input and outputs formatted text to standard output|
|grep|Searches each line of standard input, and outputs every line containing a specified pattern of characters|
|head|Outputs the first 10 lines of its input|
|pr|Takes text input from standard input and splits the data into pages (page breaks, headers, footers) in preparation for printing|
|sed|Stream editor used to perform more complicated text translations|
|sort|Sorts standard input and outputs sorted result to standard output|
|tail|Outputs the last few lines of its input|
|tr|Translates characters (casing, line termination character conversions, etc...)|
|true|Will exit successfully with a value of 0|`true`|true|
|uniq|Given a ***sorted*** stream from standard input, removes duplicate lines|

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# File Permissions
|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|su|Temporarily become the superuser. Requires superuser password. Opens a new shell as superuser|`su`|su|
|sudo|Temporarily become the superuser. Requires own password|`sudo <command>`|sudo apt get|
|chmod|Change permissions of a file or directory|`chmod <group of 3 octal digits (0-7)> <file/dir>`|chmod 777 some_file.txt|
|chown|Change ownership of a file/dir. Requires superuser privileges|`chown <desired_owner> <file/dir>`|sudo chown user2 text_file.txt|
|chgrp|Change group ownership of a file/dir. Must be owner of the file/dir to change the group|`chgrp <desired_group> <file/dir>`|chgrp group2 text_file.txt|

## **chmod**
Consider the permissions as a series of bits
|rwx|rw-|r--|
|:-:|:-:|:-:|
|111|110|100|

Find the binary representation of each group of bits
|111|110|100|
|:-:|:-:|:-:|
|7|6|4|

To change the permissions of a file or directory, use the numbers to represent permissions for the owner, group, public, respectively
```bash
# read write execute for owner, read write for group, read for everyone else
chmod 764 some_file.txt

# use shorthand to modify permissions - # + to add permissions, - to remove permissions
# add write permission for the owner of the file
chmod u+w <file>

# remove read and write permissions for the group of the file
chmod g-rw <file>

# add read permission for other users
chmod o+r <file>

# give owner full permissions, and give group and others read and execute permissions
chmod u=rwx,gr=rx <file>

# give everyone read permission
chmod a+r <file>

# remove all permissions for other users for all files in the given directory including the directory itself
chmod -R o-rwx <directory>
```

To change the owner of a file or directory, we can use the chown command. Only the root user is allowed to change the owner of a file/directory
```bash
# use the sudo command to act as the superuser
sudo chown <owner-name> <file/directory>
```

To change the group users of a file or directory, we can use the chgrp command
```bash
chgrp <group-name> <file/directory>
```

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Process Control
|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|&|Run the process in the background|`<process> &`|xload &|
|bg|Put a process in the background|`bg`|bg|
|fg|Put a process in the foreground|`fg`|fg|
|jobs|General listing of all processes running|`jobs`|jobs|
|ps|Detailed list of the processes running on the system|`ps`|ps|
|kill|Send a signal to one or more processes|`kill %<job-number/PID>`|kill %1|

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# kill Signals
A short list some signals that can be sent to processes using the `kill` command
|Signal #|Signal|Description|
|:-:|:-:|:-|
|1|SIGHUP|**Hang up** signal. Signal sent to processes when the terminal is closed|
|2|SIGINT|**Interrupt** signal. Interrupts a process (ctrl + c)|
|15|SIGTERM|**Termination** signal. Terminates the process. Default signal sent by `kill` command|
|9|SIGKILL|**Kill** signal. Immediate termination of a process. Programs **CANNOT** listen for this signal and act upon it|

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Wildcard Characters
|Symbol|Usage|
|:-:|:-|
|`*`|Match any character|
|`?`|Match any ***SINGLE*** character|
|`[characters]`|Matches any character in the set|
|`[!characters]`|Matches any character ***NOT*** in the set|

### *Wildcard command examples*
```bash
 # All filenames that begin with a g
g*

# All filenames that begin with a b and have file extension .txt
b*.txt

# Filename that begins with 'Data' followed by EXACTLY 3 characters
Data??

# Filename that begins with an 'a' or 'b' or 'c' followed by any other characters
[abc]*

# Any filename that begins with exactly one uppercase letter followed by any other characters
[[:upper:]]*

# Filename that begins with 'BACKUP' followed by EXACTLY 2 numerals
BACKUP.[[:digit:]][[:digit:]]

# ANy filename that does not end in a lowercase letter
*[![:lower:]]
```

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Posix Character Class
These can be used in place of the wildcard `[characters]` set
- [:alnum:] - Alphanumeric characters
- [:alpha:] - Alphabetic characters
- [:digit:] - Numerals
- [:lower:] - Lowercase alpha characters
- [:upper:] - Uppercase alpha characters

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

