# Table of Contents
- [Shell Scripting](#shell-scripting)
- [Environment](#environment)
	- [Startup Files](#startup-files)
	- [Paths](#paths)
	- [bin](#bin)
- [Aliases](#aliases)
- [Here Scripts](#here-scripts)
- [Shell Functions](#shell-functions)
	- [Variables](#variables)
	- [Control Flow](#control-flow)
	- [Keyboard Input](#keyboard-input)
	- [Command Separator](#command-separator)
	- [Tracing](#tracing)
	- [Positional Parameters](#positional-parameters)
	- [Logical Operators](#logical-operators)
	- [Trap](#trap)
- [Glossary](#glossary)

</br>

# Shell Scripting

___NOTE: It is now recommended to use the new [[ <expression> ]] format when formulating tests. The following examples still follow old conventions but should be replaced with the new convention `[[` `]]`___

```bash
# This is a shebang - used to determine the program that will interpret the script. Add an -x option to turn on tracing (display commands being evaluated)
#! /bin/bash

# My first script comment

echo "Hello World!"
```

# Environment

## **Startup Files**
The shell will read a set of configuration files (startup files) that define the environment

**Login Shells**
- Shell that prompts for a username and password

|File|Contents|
|:-:|:-|
|`/etc/profile`|Global configuration scripts that apply to all users in the login shell environment|
|`~/.bash_profile`|User's personal startup file. Can be used to extend or override global configuration settings in the login shell environment|
|`~/.bash_login`|Bash will read this file if the '~/.bash_profile' file is not found|
|`~/.profile`|Bash will read this file if both the '~/.bash_profile' and '~/.bash_login' files are not found. This acts as the default file|

**Non-login Shells**
- Shell that starts from a terminal session in the GUI

|File|Contents|
|:-:|:-|
|`/etc/bash.bashrc`|Global configuration scripts that apply to all users in the non-login shell environment|
|`~/.bashrc`|User's personal startup file. Can be used to extend or override global configuration settings for non-login shell environment|

## **Paths**
The shell maintains a *colon-separated* list of directories (called the `Path`) where executables are stored. When given a command without a specified pathname, the shell will search the Path list for the program (command). If the program is not found in the Path list, then a "command not found" error message will be thrown
```bash
# view the list of directories denoted by the Path
echo $PATH

# adding new directories to the pathname (temporarily)
export PATH=$PATH:<directory>
```
To include a new directory to the Path list each time the shell is started, we would need to edit the `.bash_profile` to include our *export* command. This will run the command on shell startup

## **bin**
Best practices linux distributions is to keep a `bin` directory in the `home` directory (*/home/bin*) to store our personal programs. 

</br>

[_**Back to top ^**_](#table-of-contents)

</br>


# Aliases
Create a new command which acts as a shorthand for a longer command
```bash
# command 'l' is now a shorthand for typing ls -l
alias l='ls -l'
```

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Here Scripts
Alternative method to include content that will be given to the standard input of a command
```bash
# format for 'here script'. "_EOF_" is a typical name used for the token
<command> << <token>
content to be used as commands standard input
<token>

# use <<- to ignore leading tabs for better readability
cat <<- _EOF_
	<html>
	<head>
		<title>
		</title>
	</head>

	<body>
	</body>
	</html>
_EOF_

```

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Shell Functions
Used to define aliases with more complex commands 
```bash
today () {
	echo -n "Today's date is: "
	date +"%A, %B %-d, %Y"
}
```

## **Variables**
We can create variables in our shell scripts using the following format `<variable-name>=<value>`, no spaces are allowed. By convention, we name constants with *UPPER* casing, and normal varialbes with *lower* casing. To utilize our variables, we use parameter expansion: `$<variable-name>`
```bash
title="My System Information"

cat << _EOF_
	<html>
	<head>
		<title>
		$title
		</title>
	</head>
	...
_EOF_
```
We can also use command substitution and assign the output to a variable.
```bash
right_now="$(date +"%x %r %Z")"
time_stamp="Updated on $right_now by %USER
```

Variables can also have no value assigned to them
```bash
# This is correct syntax
number=

# Note, to evaluate an empty variable as an empty string ("") we need to use double quotes during expansion
if [ "$number" = "1" ] # this will read as - if [ "" = "1" ]

# If we do not include the double quotes, an error will be raised. In this case, there will be a unary operator error because $number evaluates to nothing
if [ $number = "1" ] # this will read as - if [ = "1" ]
```

Note: When using parameter expansion or command substitution, wrap the expression in **double quotes** to avoid problems where the output has desirable whitespace characters

## **Control Flow**
|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|if|Makes decision based on *exit* status of a command|`if <commands>`|if [ -f .bashrc ]|
|fi|Close the conditional if statement|`fi`|if true; then echo "true"; fi|
|test|Evalutes if an expression is true(0) or false(1). There **must** be a leading and trailing whitespace between the expression and the brackets|`[ expression ]`|[ -f .bashrc ]|
|exit|The script will terminate immediately and the exit status value will be set to the given value|`exit <value>`|exit 1|

You can check the exit status value of the previous command by using `echo $?`. Some commands will output a specific integer value (between 0-255) to give meaning to an error. You can check the MAN pages of the command to see the exit status value meaning

___NOTE: It is now recommended to use the new [[ <expression> ]] format when formulating tests. The following examples still follow old conventions but should be replaced with the new convention `[[` `]]`___

```bash
if [ -f .bashrc ]; then
	echo "You have a .bashrc. Things are fine."
else
	echo "You have no .bashrc"
fi

# multi-branching
if [ "$character" = "1" ]; then
	echo "1"
elif [ "$character" = "2" ]; then
	echo "2"
else
	echo "0"
fi
```

```bash
# check if the current user's id value is equal to the string "0" (superuser)
if [ "$(id -u)" != "0" ]; then
	echo "You must be the superuser to run this script" >&2 # >&2 will output our message to STANDARD ERROR (I/O redirection)
	exit 1
fi
```

We can use the `case` command to write multi-branch logic

|Command|Description|Format|
|:-:|:-|:-:|
|case|Selectively executes statements if expression matches a pattern|`case word in patterns ) commands ;; esac`|
|esac|Ends the case statement|`case ... esac`|

```bash
case <expression> in 
	[[:lower:]] | [[:upper:]] ) echo "You entered a character"
				    ;;
	[0-9] ) 		    echo "You entered a number"
	    			    ;;
	* ) 			    echo "Oops"
esac
```

We can write loops to repeat certain sections of our script

|Command|Description|Format|
|:-:|:-|:-:|
|while|Will repeatedly execute a block of code based on exit status of given command|`while [ <expression> ]; do <commands> done`|
|done|Ends the while loop statement|`while ... done`|
|until|Works like the `while` command, but will repeat only if the exit status of the given command is ***false***"|`until [ <expression> ]; do <commands> done`|
|for|Loops through each item in a list of words|`for <variable> in <list>; do <commands> done`|

```bash
# While loop example (-lt = less than)
while [ "$number" -lt 10 ]; do
	echo "Number = $number"
	number=$((number + 1))
done

# until-loop example (-ge = greater or equal)
until [ "$number" -ge 10 ]; do
	echo "Number = $number"
	number=$((number + 1))
done

# for-loop example
for i in $(cat ~/.bash_profile); do
	count=$((count + 1))
	echo "Word $count ($i) contains $(echo -n $i | wc -c) characters"
done
```

## **Keyboard Input**
|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|read|Get input from the keyboard and assign it to a variable (REPLY env var if none given)|`read <variable>`|read text|

```bash
# -p flag allows us to add text prompt
-p "Enter some text > " text

# -n flag will timeout after a given number of seconds if no input is given
if read -t 3 response; then
	echo "Thank you for the input"
else
	echo "Sorry, time has run out"
fi

# -s flag will hide the input as the user is typing

-ps "Please enter your password" read password
```

## **Command Separator**
A semicolon(;) is used as a command separater, allowing us to put more than one command on a line
```bash
if true; then echo "It's true"; fi

clear; ls
```

## **Tracing**
To turn **on** tracing, we can use the command `set -x` before the commands we want to trace. To turn **off** tracing, we can use the command `set +x` after the commands we want to trace
```bash
set -x
if [ $number = "1" ]; then
	echo "Number equals 1"
fi
set +x
```

## **Positional Parameters**
Special variables ($0 - $9) that contain the contents of the command line. After the 9th argument, subsequent arguments can be accessed using the following notation `${<argument#>}`
```bash
program arg1, arg2, arg3 ... arg10

# $0 = program
# $1 = arg1
# $2 = arg2
# $3 = arg3
# ${10} = arg10
```
The special variable `$#`contains the *number* of items on the command line
The special variable `S*` and `S@` denote *all* the positional parameters

```bash
if [ $# -gt 0 ]; then
	echo "Your command line contains $# arguments"
fi
```

The `shift` command lets us iterate over positional parameters. The positional parameters are shifted down one position (#2 becomes #1) on each call of `shift`

```bash
while [ "$1" != "" ]; do
	echo "Paramater 1 equals $1"
	shift
done
``` 

Using a for-loop we can easily read the positional parameters as a list denoted by `$@`
```bash
for i in "$@"; do
	echo $i
done
```

List of function parameters
|Parameter|Description|
|:-:|:-|
|`$0`|Name of the script|
|`$1-$9`|Arguments to the script|
|`$@`|All the arguments|
|`$#`|Number of arguments|
|`$?`|Return code of previous command|
|`$$`|PID number for current script|
|`!!`|Entire last command including arguments|
|`$_`|Last argument from the last command|


## **Logical Operators**
The AND `&&` operator will only execute the next command iff the first command's exit status is successful (0)
```bash
command1 && command2
```
The OR `||` operator will execute the next command iff the first command returns a non-zero exit status
```bash
command1 || command2
true || echo "This will output" # The echo command will not output in this case
```

## **Trap**
The trap command lets us handle situations where our program is sent an unexpected signal
```bash
trap "rm $tempfile; exit" SIGNINT SIGTERM
```
SIGKILL cannot be handled by a program and will always terminate any executing program. Use SIGKILL as a last resort

</br>

[_**Back to top ^**_](#table-of-contents)

</br>

# Glossary
|Term|Definition|
|:-:|:-|
|shebang|construct to indicate to the system which program is to be used to interpret the script|
|Path|A list of directories where executable files are stored|
|export|command to tell the shell to make the exported content available to child processes of the shell|
|exit status|value given to the system when a command terminates. By convention, 0 is considered successful, and any other integer is considered a failure. The exit status value can range from any integer between 0-255 inclusive|

</br>

[_**Back to top ^**_](#table-of-contents)

</br>
