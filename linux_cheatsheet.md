# Table of Contents
- [Linux](#linux)
- [Linux Directories](#linux-directories)
- [Command Types](#4-command-types)
- [I/O Redirection](#io-redirection)
   - [Standard Output](#standard-output)
   - [Standard Input](#standard-input)
   - [Pipelines](#pipelines)
- [Expansions](#expansions)
	- [Arithmetic Expansion](#arithmetic-expansion)
	- [Brace Expansion](#brace-expansion)
	- [Parameter Expansion](#parameter-expansion-variable-expansion)
	- [Command Substitution](#command-substitution)
- [Quoting](#quoting)
   - [Double Quotes](#double-quotes)
   - [Single Quotes](#single-quote)
- [Escape Characters](#escape-characters)
- [Long Format](#long-format)
- [Glossary](#glossary)

</br>

# Linux
- Will ***always*** have one tree (_drives are considered subtrees_)
- Linux does not care for file extensions


# Linux Directories
|Directory|Description|
|:-:|:-|
|`/`|The root directory|
|`/boot`|Contains the linux kernel and boot loader|
|`/etc`|Contains the system configuration files (text files)|
|`/bin /usr/bin`|The bin directory contains essential programs required by the system to operate. The usr/bin directory contains user-applications|
|`/sbin /usr/sbin`|Contains programs for system administration (superuser)|
|`/usr`|Contains supporting material for user-applications (manuals, docs, file-format supports)|
|`/usr/local`|Outisde applications (*not part of the official distribution*) are installed here, usually the `/usr/local/bin` directory|
|`/var`|Contain files that change as the system is running (log files, spools for queue jobs)|
|`/lib`|Location of shared libraries|
|`/home`|Typically contains User's personal work where they have permission to write files|
|`/root`|Superuser's home directory|
|`/tmp`|Programs write their temporary files here|
|`/dev`|Contains devices that are available to the system|
|`/proc`|Virtual directory that contains entires corresponding to all the processes running in the system|
|`/media`|Mount points (phsyical storage devices)|

</br>

[***Back to top ^***](#table-of-contents)

</br>

# 4 Command Types
- Executable Program
- Builtin commands - terminal commands (cd, ls, etc...)
- Shell functions - environment configuration
- Alias - custom commands created using other commands

</br>

[***Back to top ^***](#table-of-contents)

</br>

# I/O Redirection

## **Standard Output**
Command line programs send their results to **standard output**, which by default displays them on the terminal window. To have the command line programs write to a file, we use the `>` character
```bash
# overwrite the contents of file_list.txt with the output of ls
ls > file_list.txt

# appends the output of ls to the contents of file_list.txt
ls >> file_list.txt
```

## **Standard Input**
Some commands can accept input from **standard input**, which by default listens for keyboard inputs. To have the command line programs use input from a file, we use the `<` character
```bash
# sort command receives input from file and outputs to console
sort < file_list.txt

# sort command receives input from file and outputs to separate file
sort < file_list.txt > sorted_file_list.txt
```

## **Pipelines**
Feed standard output of one command into the standard input of another command
```bash
# open the file list using less for easy navigation
ls -l | less
```

|Command|Description|
|:-:|:-|
|ls -lt `\|` head|Displays the 10 newest files in the current directory|
|du `\|` sort -nr|Displays list of directories and their sizes, sorted from largest to smallest|
|find . -type f -print `\|` wc -l|Displays the total number of files in the current working directory and all of its subdirectories|

</br>

[***Back to top ^***](#table-of-contents)

</br>

# Expansions

## **Arithmetic Expansion**
Basic *integer* arithmetic can be done in the shell. Format of expansion is as follows `$((expression))`, spaces are ignored. Overflow will occurr if out of range.
```bash
echo $((2 + 2)) # will output 4
echo $((10 - 15)) # will output -5
echo $(( 15 * 15 )) # will output 225
echo $((5/2)) # will output 2
echo $((5%2)) # will output 1
echo $(((5**2) * 3)) # nested subexpressions need only one set of parenthesis - outputs 75
```

## **Brace Expansion**
Create multiple text strings. Brace expression can be a comma-separated list or a range of numerals/characters
```bash
echo Front-{A,B,C}-Back # outputs Front-A-Back Front-B-Back Front-C-Back
echo Number_{1..5} # outputs Number_1 Number_2 ... Number_5
echo Letter_{Z..A} # outputs Letter_Z Letter_Y ... Letter_A
echo a{A{1,2},B{3,4}}b # outputs aA1b aA2b aB3b aB4b
mkdir {2017..2019}-{01..12} # outputs a set of directories in Year-Month format from 2017-01 2017-02 ... 2019-11 2019-12
```

## **Parameter Expansion (Variable Expansion)**
Way to store chunks of data. Paramters can be invoked using the following format `$<Parameter-name>`. To see a list of parameters use the following command `printenv | less`
```bash
echo $USER # outputs the username

echo ${1:-"unknown error"} # if $1 is undefined, use the right-hand expression
```

## **Command Substitution**
Use the output of one command as an expression. The format for command substitution is as follows `<command> $(<command>)` or replace `$()` with a pair of backticks
```bash
ls -l $(which cp) # outputs the file details for the 'cp' file without needing the full pathname
ls -l `which cp` # outputs same as above
file $(ls /usr/bin/* | grep bin/zip) # outputs type of file for files named with some form of `bin/zip` 
```

</br>

[***Back to top ^***](#table-of-contents)

</br>

# Quoting

## **Double Quotes**
Adding double quotes around expressions allows us to suppress some shell functions ***EXCEPT*** parameter expansion, arithmetic expansion, and command substitution. Double quotes allow us to keep whitespace characters when evaluating command substitutions
```bash
ls -l "two words.txt" # allows us to handle files/directories containing whitespace in their names
echo "$USER $((2+2)) $(cal)" # does not suppress the following expressions, output will be as expected

echo $(cal) # will output the calendar in a single line (echo will execute with 38 arguments)
echo "$(cal)" # will output a formatted calendar (echo will execute with 1 argument)
```

## **Single Quote**
To suppress ***ALL*** shell expressions, we can wrap expressions in single quotes
```bash
echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER # ouputs text <all-files-in-home-directory-ending-in-.txt> a b foo 4 <username>
echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER" # ouputs text ~/*.txt {a,b} foo 4 <username>
echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER' # outputs text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

</br>

[***Back to top ^***](#table-of-contents)

</br>

# Escape Characters
To escape a single character, we can user the backslash `\` character before any special character, including the backslash character itself `\\`. Backslash loses its special use when placed in an expression surrounded by single quotes.
```bash
echo "The $USER has \$5.00" # outputs The <username> has $5.00
mv bad\&filename good_filename # rename the filename containing a special character (in this case '&' is a special character) to a more proper filename

# below is an example of using the backslash to escape newline characters for commands that have a long list of arguments
ls -l \
   --reverse \
   --human-readable \
   --full-time

```

|Escape Character|Name|Description|
|:-:|:-:|:-|
|\n|newline|Add a blank line|
|\t|tab|Insert horizontal tab|
|\a|alert|Makes a beep noise|

</br>

[***Back to top ^***](#table-of-contents)

</br>

# Long Format
```bash
# Example of ls -l
-rwxrwxrwx 1 me me 879 Dec  7 14:23 command_cheatsheet.md
```
|File permissions|Owner|Group|Size (bytes)|Modification Time|File Name|
|:-:|:-:|:-:|:-:|:-:|:-:|
|First character represents type of file (`-` represents ordinary file. `d` represents a directory. `l` represents symbolic link). Every 3 characters after represents the `read, write, execution` rights for the file's `owner`, file's `group`, and the `public` access rights, in that order|Name of the user that owns the file/directory|Name of the group that also has file permissions|Size of the file in bytes|Timestamp of when the file was last modified. Date and year are shown if last modification was more than six months ago, otherwise time of day is shown|Name of the file/directory|  
|-rwxrwxrwx 1|me|me|879|Dec 7 14:23|command_cheatsheed.md|

## **File**
- read (r) - read the contents of the file
- write (w) - write to the file
- execute (x) - run the file like a program

## **Directory**
- read (r) - allows content of directory to be listed if the 'x' permission is set
- write (w) - allows creation, deletion, renaming of files within the directory if the 'x' permission is set
- execute (x) - allows the directory to be entered

</br>

[***Back to top ^***](#table-of-contents)

</br>

# Glossary
|Term|Definition|
|:-|:-|
|Current Working Directory|The directory you are in|
|Absolute Path|Pathname begins with the root directory|
|Relative Path|Pathname begins at the current working directory|
|Symbolic Link|File that points to another file|

</br>

[***Back to top ^***](#table-of-contents)

</br>
