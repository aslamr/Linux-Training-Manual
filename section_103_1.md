# Ict-innovation/LPI/103.1
### **Contents:**
#### 1. 103.1 Working on the Command Line
#### 1.1 [Overview](#overview)
#### 1.2 [The Interactive Shell](#the-interactive-shell)
#### 1.3 [Shell Variables](#shell-variables)
#### 1.4 [Wildcards](#wildcards)
#### 1.5 [The Commnad History](#the-command-history)
#### 1.6 Manpages and the whatis database   
  
---
### **1. 103.1 Working on the Command Line**
#### *Candidates should be able to interact with shells and commands using the command line. The Objective assumes the bash shell.*
  

#### **Key Knowledge Areas**
- Use single shell commands and one line command sequences to perform basic tasks on the command line.
- Use and modify the shell environment including defining, referencing and exporting environment variables.
- Use and edit command history.
- Invoke commands inside and outside the defined path.  
 --- 
### **Overview**
A basic way to interact with a computer system is to use the command line. The shell interprets the instructions typed in at the keyboard. The shell prompt (ending with $ or # for user root) indicates that it is ready for user input.

The shell is also a programming environment which can be used to perform automated tasks. Shell programs are called scripts.

| **Most Common Shells** | |
| :------ | :------|
| The Bourne Shell | /bin/sh |
| The Bourne Again Shell | /bin/bash |
| The Korn Shell | /bin/ksh |
| The C Shell | /bin/csh |
| Tom's C Shell | /bin/tcsh |

Since the bash shell is one of the most widely used shells in the Linux world the LPI concentrates mainly on this shell.

---
### **The Interactive Shell**
Shell commands are often of the form

**command [options] {arguments}.**

The bash shell uses the echo command to print text to the screen.

```bash
$ echo "this is a short line
```
  
**Full/Relative Path**
The shell interprets the first ¨word¨ of any string given on the command line as a command. If the string is a full or relative path to an executable then the executable is started. If the first word has no ¨/¨ characters, then the shell will scan directories defined in the PATH variable and attempt to run the first command matching the string.

For example if the PATH variable only contains the directories /bin and /usr/bin then the command ~~xeyes~~ won't be found since it is stored in ~~/usr/X11R6/bin/xeyes~~ so the full path needs to be used.

```bash
$ /placeholder/placeholder "Olwethu Needs to update this"
```
An alternative to typing the full path to an executable is to use a **relative** path. For example, if the user is in the directory where the ~~xeyes~~ program is stored then one can type.

```bash 
$ ./placeholder "Olwethu Needs to update this"
```
The '.' in the above command means the current working directory. Since you are already in the current directory the command could also simply be written as: (only if “.” is in your search path)

```bash 
$ placeholder "Olwethu Needs to update this"
```
---
### **Shell Variables**
Shell variables are similar to variables used in any computing language. Variable names are limited to alphanumeric characters. For example CREDIT=300 simply assigns the value 300 to the variable named CREDIT.
| **Define Variables**| |
| :------ | :------ |
| 1. Initialise a Variable: | Variable-Name=value (no spaces!!)|
| 2. Reference a Variable: | $Variable-Name |

```bash
CREDIT=300
echo $CREDIT
```
The value of a variable can be removed with the **unset** command.
  

**Export, Set and Env**

There are two types of variables: local and exported.

Local variables will be accessible only to the current shell. On the other hand, exported variables are accessible by both the shell and any child process started from that shell.

The commands **set** and **env** are used to list defined variables.

| *The set and env commands*| |
| :------ | :------ |
| **set** | Lists all variables |
| **env** | List all exported variables |
  

A global variable is global in the sense that any child process can reference it. The example below exports the variable credit and then checks to see if it has been exported as expected.

```bash
export CREDIT
env | grep CREDIT
```
  
List of common predefined variables

| PREDEFINED VARIABLES | MEANING |
| :------ | :------ |
| DISPLAY | Used by X to indentify where to run a client application |
| HISTFILE | Path to the user's .bash_history file |
| HOME | The path to the user's home |
| LOGNAME | The name used by the user to log in |
| PATH | List of directories searched by the shell for programs to be executed when a command is entered without a path.|
| PWD | The current working directory |
| SHELL | The shell used (bash in most Linux distributions) |
| TERM | The current terminal emulation |
  
**Special Variables**
The next few variables are related to process management.
  

```bash
echo $! "represents the PID value of the last child process"
echo $$ "represents the PID of the running shell"
echo $? "is 0 if the last command was executed successfully and non-zero otherwise"

*"Olwethu added echo before the variable as the command doesn't execute successfully without it in openSUSE 15.4"*
```
**Metacharacters and Quotes**

Metacharacters are characters that have special meaning for the shell. They are mainly used for file globbing, that is to match several files or directory names using a minimum of letters. The input (<), output (>) and pipe (|) characters are also special characters as well as the dollar ($) sign used for variables. We will not list them here but note that these characters are seldom used to name regular files.

---
### **Wildcards**
- The * wildcard can replace any number of characters.

```bash 
$ ls /usr/bin/b*
```
lists all programs starting with a 'b'

- The ? wildcard replaces any one character.

```bash
ls /usr/bin/?b*
```
lists all programs having a 'b' as the second letter
  
- '[ ]' is used to define a range of values.

```bash
$ ls a[0-9]
$ ls [!Aa]*

"need to review the first command, doesn't print a list to the terminal, echo seems to work better than ls" 

"Adding an asterix at the end of the first command might produce an output that makes this descriprion below easier to understand" 

```
First line lists all files starting with an 'a' and have a digit in second position.
  
The second line lists all files that don't start with an 'a' or an 'A'
  
- {string1,string2} although not a file naming wildcard, it can be used to generate a list of names that have a common stem.
  
```bash
$ mkdir {mon,tues, wednes}day

```
**Quotes and escape codes**

The special meaning of metacharacters can be cancelled by escape characters, which are also metacharacters.
  
The backslash (\) is called the **escape character** and cancels the meaning of the following character, forcing the shell to interpret it literally.
  
The single quotes (' ') cancel the meaning of all metacharacters except the backslash.

The double quotes (" ") are the weakest quotes but cancel most of the special meaning of the enclosed characters except the pipe (|), the backslash (\) and a variable ($var)

**The Back Tick**

Back quotes `` will execute a command enclosed and substitute the output back on the command line. The next example defines the variable TIME using the date command.

```bash
$ TIME="Today's date is 'date +%a.%d.%b'"
echo $TIME

Today's date is Sun:15:Jul
```
Another way of executing commands (similar to the back ticks) is to use **$()**. This will execute the enclosed command and treat it as a variable.
  
```bash
$ TIME=$(date)
```
---
### **The Command History**
To view the list of previously typed commands you can use the **bash** built-in command history.
  
```bash
$ history

1. ls
2. grep 500/etc/passwd
```
  
This has listed all the cached commands as well as the commands saved in **~/.bash_history**. When a user exits the shell cached commands are saved to **~/.bash_history**.

You can recall commands by using the Up-arrow and Down-arrow on your keyboard. There are also emacs key bindings that enable you to execute and even edit these lines.
| **Emacs Key Bindings for Editing the Command History**  | |
| :------ | :------|
| Ctrl+P | Previous line (same as Up-arrow) |
| Ctrl+| Next line (same as Down-arrow) |
| Ctrl+b | Go back one character on the line (same as Left-Arrow) |
| Ctrl+f | Go forward one character on the line (Same as Right-Arrow) |
| Ctrl+a | Go to the beginning of the line (Same as 'Home') |
| Ctrl+e | Go to the end of the line (Same as 'End') |

The **bang (!)** key can be used to rerun a command.
```bash  
$ ! "runs the last command"
$ !x "executes the latest command in the history list starting with an 'x'"
$ !2 "runs command number 2 from the history output"	
$ !-2 "runs the command before last"
$ !^string1^string2 "run previous command and replace string1 by string2"

"All commands work as expected except for line number 1, needs to be looke at"
```
**Other Commands**

**Aliases**

You can create aliases for commands needing many arguments. The format to create an alias is:
```bash
$ alias myprog='Command [options]{arguments}'
```
By typing alias alone at the command line, you will get a list of currently defined aliases.
  
By pressing **TAB**, the shell will complete the commands you have started typing in.
  
**Compound Commands**
| Compound Commands| |
| :------ | :------|
| command1 ; command2 ; command3 | The three commands are run in sequence regardless of the success of the previous command |
| command1 && command2 && command3 | Each command will execute only if the previous exit code is 0 (success) |
| command1 \|\| command2 \|\| command3 | The next command will execute only if the previous exit code is not 0 (failure) |

**The "exec” command**

This command is not a binary but rather is part of the shell. It is used to start other commands. Ordinarily if a command is executed, a sub-process is started. If the exec command is used to initiate the new program, it reoccupies the process used to start it. It replaces the current shell (in a script or the interactive shell).

When the new command terminates, control is not passed back to the calling shell, but returns to the process that called the shell used to make the exec call.