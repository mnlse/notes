UNIX/Linux  
Author: Matthew Nielsen  
License: [Creative Commons](https://creativecommons.org/licenses/by/4.0/)  
# UNIX Principles
1. Everything is a file.
2. File extensions mean nothing (except information for the user).
3. Anything with a tilda(~) suffix is a backup.
4. Programs are designed to work with other programs: one program's output is another program's input

# UNIX Basics
Shell - a program that allows you to make basic use of your system (like traversing directories,
        moving files) and executing other programs. If you execute another program through
        shell, it delegates control of the system to that program.


Programs in UNIX are designed to work together. Output of any program can be used as input to another.
This means you can chain your programs together in different combinations to achieve complex objectives.

Daemon - background process that makes using the computer easy. Examples:
         handling the printer, keeping track of the time, checking for updates etc.


If you shut down your system forcefully, you might damage your system, because of the daemons
running. To shut it down properly, use the `shutdown` command.

UNIX runs a checkup everytime it boots up, called `fsck` - filesystem check, just in case
you haven’t switched off your system properly and fixes any problems that might have arisen
out of your mistake.

# Files
## Directory Structure
`/bin` - executables  
`/boot` - location of the kernel, loaded by bootloaded,  
        commonly separate filesystem on the disk  
`/cgroup` - kernel abstraction, not a standard        
`/dev` - location of devices  
`/home`  
`/lib` - library locations  
`/lib64`  
`/media` - removable media  
`/mnt` - temporary mounts  
`/opt` - optional software  
`/proc` - pseudo filesystem that exposes kernel config and process config  
`/root` - special dir for root user (home dir)  
`/sbin` - sensitive binary files that only root can use  
`/tmp` - storing temporary files (you can use it too)  
         In Ubuntu /tmp is removed on reboot  
`/usr` - shared user data  
`/var` - variable data files, sometimes databases put files here  
       the point of this is that the files here are constantly  
       changing

## Unix File Types
1. Links  
   • symbolic  
   • hard  
2. Executable  
3. Socket  
      A socket is a file that allows UNIX processes to communicate efficiently.  
4. Named pipe  
      A named pipe is a *pipe that has its own file, and filename*
      The tool to create named pipes is *mkfifo*

      Example:  
      ~~~
      mkfifo my_pipe
      gzip -9 -c < my_pipe > out.gz &
      ~~~
5. Door
      A door is another inter-process communication method.

6. Directory

## atime, ctime, mtime
1. atime - last time FILE was accessed  

2. ctime - last time FILE was changed  
      A change is when you change the file’s permissions or ownership.  

3. mtime - last time FILE was modified  
      A modification is when you change the contents of a file.  

# Bash
## Programming
### Intro
Single line comment: `#`

You have to tell Linux which interpreter to use for your script. For bash use:
~~~
#!/bin/bash
~~~

### Variables
Spaces are not allowed.

Use `${#variableName}` to count the number of characters in a word.

~~~bash
#!/bin/bash                                        
echo -n "Enter your word: "
read word
echo "The word $word contains ${#word} characters"
~~~

Default variables:
if a variable is not set by the user, you can specify a default value

There are two ways of doing it:
~~~
if [ -z $var ]
then
   $var=value
fi

~~~
or:

`: ${$var:=value}`

We have to put the colon, or “value” will be interpreted as a command.


`$1, $2, $3,` ... are the positional parameters.  
`“$@”` is an array-like construct of all positional parameters, {$1, $2, $3 ...}.  
`“$*”` is the IFS expansion of all positional parameters, $1 $2 $3 ....  
`$#` is the number of positional parameters.  
`$-` current options set for the shell.  
`$$` pid of the current shell (not subshell).  
`$_` most recent parameter (or the abs path of the command to start the current shell immediately after startup).  
`$IFS` is the (input) field separator.  
`$?` is the most recent foreground pipeline exit status.  
`$!` is the PID of the most recent background command.  
`$0` is the name of the shell or shell script.  


### Decimals
~~~
echo $(( 2#111 ))
          ^ number 7(seven) in binary numer system
~~~          

This will convert the number seven in binary numer system to seven in decimal numer system

~~~bash
echo $(( 2#111 ))  
=>7
~~~

### Colons
Semicolon is useful when I want to put more than one command in one line.

~~~
echo “put in something”; read input
~~~

A colon “:” means “do nothing”.

~~~
if diff file1 file2 > /dev/null
then
   :        # *do nothing*
else
   echo “Difference(s) found”
   exit
fi   
~~~
### Comparasion
~~~bash
var=10                    
                       
if [ $var -gt 5 ]       
then                      
echo ‘$var is gt than 5’  
fi                        
~~~

You can use traditional notation using two curly braces.
~~~bash
 var=10              
 if (( $var > 5 )) 
 then                
     echo yes            
 fi                  

 => yes
~~~


Note: you don’t have to put semicolons after “then”

1. `-eq`  
   Equal to

2. `-ne`  
   Not equal to

3. `-gt`  
   Greater than

4. `-ge`  
   Greater than/equal to

5. `-lt`  
   Less than

6. `-le`  
   Less than/equal to

7. `-a`  
   Logical AND

8. `-o`  
   Logical OR

To use traditional comparasions use double parentheses.
~~~bash
(( “k” == “k” ))
(( 3 > 4 ))
~~~     


### Conditional operators
#### if

~~~bash
if command1
then
    # code block
fi
~~~

The if commands evaluates to *true* if `command1`’s `$? = 0` (true)

You can add an else statement to account for exit status other than 0:

~~~bash
if command1
then
    # code block1
else
    # code block2
fi
~~~

To catch exit codes other than 1 and 0:
~~~bash
command1
if [ $? -eq 0 ]
then
    # code block1
elif [ $? -eq 4 ]
    # code block2
else    
    # code block3
fi
~~~

#### elif

If there are more than 2 mutually exclusive blocks, use the elif statement

~~~bash
if command1 
then
    # code block1
elif        
    # code block2
else        
    # code block3
fi          
~~~

#### case

~~~bash
case $var1 in
   val1)
        # code block1
      ;;
   val2)
        # code block2
      ;;
   val3)
        # code block3
      ;;
esac
# ^ case spelled backwards ends the block
~~~

If “val” has two words, use quotation marks.

~~~bash
“Dire Straits”)
        # code block
      ;;
~~~


**The default statement in bash is** * (asterisk)

~~~bash
case $x in
   var1)
        # code block1
      `;;`
   *)      # Default statement
        # code block2
      `;;`
~~~

We can use the logical or statements in case tests:
~~~bash
   var1|var2|var3)
        # code block
      `;;`
~~~

Also, we can use wildcard characters.


# UNIX POWER TOOLS

### Filenames

There is a limit on filename length, even in modern implementations.

You can use just about any character, except a slash (/).
As a good practice, avoid using any crazy filenames. Stick to these conventions:
1. A-z  
2. underscores (`_`) ← separating words  
3. periods (`.`)  ← separating filenames and file extensions  
4. commas (`,`) ← user whim  

Unix as a system doesn’t have any special rules about extensions (as opposed to MS Windows), however
it many of its programs make use of extensions.

Examples of extensions:  
`.html   .tar   .tar.gz   .shar (shell archive)   .sh (Bourne shell script)   .csh (C shell script)
.ps (PostScript source file)   .pdf (Adobe Portable Document Format)`

Dot hidden files:
If you put a dot (`.`) in front of a filename, it is considered a special file that is not shown
by programs, unless you specifically ask them to. It is a hidden file.

## Working with file names
### Wildcards

`?` - match any character (1 character)  
`*` - match zero **or more** characters  
`[abcde]` - match the specified characters in the brackets (in this case match a,b,c,d,e)
          `[a-Z]` matches all lowercase alphabetic characters and all uppercase alphabetic characters  
          `[0-9]` match numbers from 0 to 9  
           `-` is the *consecutive characters* operator that takes two operands


## Permissions
Permissions are made for multi-user systems for user separation, and for security reasons.

There are two tools to change permissions - `chown` and `chgrp`. Chgrp is a tool specially designed
to change the *group that owns a file* By default, the group that owns a file is the main group
of the user (that created the file).

## Executables
### whatis, whereis
`whatis` - display simple info message on what a program does

Example:
~~~bash
whatis ls
# => ls (1)               - list directory contents
~~~

`whereis` - display the *location*, *source code*, *man pages*  
Example:
~~~bash
whereis cat
# => cat: /bin/cat /usr/share/man/man1/cat.1.gz
#           ^ location      ^ man pages
~~~     

Source in this case is missing. 

Options of “whereis”:

   `-b` - *executable name*  
   `-m` - *manual page*  
   `-s` - *source files*  
   `-u` - report only if requested information (executable, manual, source page) is missing  

### Find version of an executable
If you want to know which version of an executable you’re using, use the *type* command.

~~~bash
type less
# => less is /usr/bin/less
~~~

~~~bash
type -all less
# => less is /usr/bin/less
# => less is /bin/less
~~~

`type less` shows you which executable will run once you execute “less” in your shell.  
`type -all less` shows you which executables there are in your $PATH, regardless of their precedence  

The difference between `type` and `which` is that `which` always shows the absolute path of where
a thing is, whereas `type` shows you when a command is bound to an alias.

~~~bash
type ls
# => ls is aliased to `ls --color=auto'`
~~~

~~~bash
which ls
# => /bin/ls
~~~

See? which always shows a path.

### Identifying what’s happening on your machine
`tty` - see which tty you’re on  
`who` - show all logged in users, and their respective ttys  

### Logging in
UNIX starts the `login` program, which performs security stuff. 

Login checks `/etc/nologin` (the file that indicates that the system is down for maintenance).  
If `/etc/nologin exists`, UNIX will not login.

Login makes logs in `/var/log/lastlo` , `/var/log/wtmp` (this file shows that you've logged in).

If the file `.hushlogin` exists in your $HOME, the login will be quiet.

Login can show a MOTD message from `/etc/motd`. After all of this, login will start a shell.
That shell is specified in `/etc/passwd`.
~~~
robsp:x:1000:1000:Maximilien Robespierre,,,:/home/robsp:/bin/bash
                                                         ^ shell
~~~                                                

### Login shell
When you login to UNIX, the first thing that’s started is a *login shell*. It’s the shell
that sets all the `$ENV` variables, `init` files, execute X11 etc.

A bash login shell reads `$HOME/.profile`, and `/etc/profile`. 

The flags of the current shell are stored in the `$-` variable.

Flags:
`i` - interactive  
`m` - monitor mode  
`H` - history substitution  

#### Setup files for shells
Elements of a setup file:  
• $PATH  
• terminal type, terminal settings  
• environment variables  
• autostart commands  

If you want to change your login shell settings, keep a way to *revert the changes you made*
in case you’ve made a mistake. You might not be able to log in if you screw up.

In shells configs, you have to use absolute pathnames.

##### Creating/modifying your own setup file
You might want to test `$TERM`, and execute different commands for different terminals.

Example:
~~~ bash
case “$TERM” in
   xterm)
       # (.... cmds ....)
   ;;
   st-term)
       # (.... cmds ....)
   ;;
esac
~~~

Testing port:  
Certain terminal port (tty) numbers are used for certain kind of logins.

`tty[pqrs]?` is used for `rlogin` and `telnet`.

Example: `ttyp1, ttyq2, ttys3, ttyr5` (etc ...)

~~~bash
case “`tty`” in
/dev/tty[pqrs]?)
      # ( commands for rlogin & telnet )
   ;;

/dev/tty02)
      # ( commands for normal login, terminal on my desk )
   ;;
esac
~~~

Checking terminal variables:  
Different system configurations create different environment variables.
(in other words: different environments create different enrivonment variables)

It might be useful to check what kind of system you’re in. Does it have X Window Manager?  
Check the `$DISPLAY` variable. If a system doesn’t have X, it won’t have a `$DISPLAY`.

Is it Mac OS X? Check `$DARWIN_VERSION` , only MAC OS X would have that variable. If it doesn’t exist,
it’s not a Mac.

~~~bash
if [ -n “$DISPLAY” ]; then
    # ...
else
    # ...
fi   
~~~

### RC files
A `rc file` is a setup file for a program.

Today, most complex programs have their own setup subdirectory (example: ~/.config/mozilla/)
But simple programs are configurable by a .rc file in your home directory.

Your system has its own RC files, located in `/etc/rc?.d`. These RC files are simply shell scripts.

## Man pages 
Man pages are structured in this way:

   `NAME`
        The program’s name; one line summary of what it does.

`   SYNOPSIS`
        How to invoke the program, including all arguments and
        command-line options. (Optional arguments are placed in
        square brackets.)

 `  DESCRIPTION`
        A description of what the program does—as long as
        is necessary.

`   OPTIONS`
        An explanation of each option.

 `  EXAMPLES`
        One or more examples of how to use the program.

`   ENVIRONMENT`
        Any environment variables that control the program’s behavior.

 `  FILES`
        Files the program internals will read or write. May include
        temporary files; doesn’t include files on the command line.

`   BUGS`
        Any known bugs. The standard manual pages don’t take
        bug recording seriously, but this can be very helpful.

 `  AUTHOR`
        Who wrote the program.


To write a man page, you have to use stuff like *troff*, which has its own special formatting.
You need to learn macros like in LaTeX to format a man page with troff.

### Example of a man page formatted with troff:
~~~bash
.TH CORRUPT 1
.SH NAME
corrupt \- modify files by randomly changing bits
.SH SYNOPSIS
.B corrupt
[\fB\-n\fR \fIBITS\fR]
[\fB\-\-bits\fR \fIBITS\fR]
.IR file ...
.SH DESCRIPTION
.B corrupt
modifies files by toggling a randomly chosen bit.
.SH OPTIONS
.TP
.BR \-n ", " \-\-bits =\fIBITS\fR
Set the number of bits to modify.
Default is one bit.
~~~
As you can see, this man page follows the basic skeleton of every man page, mentioned earlier.

## Shell customization
### Colors
To add a color to PS1:  
`\e[m` : Start color scheme.  
`x;y` : Color pair to use (x;y)  
`$PS1` : Your shell prompt variable.  
`\e[m` : Stop color scheme.  

This goes as follows
1. Start color scheme: `\e[m`
2. Define colors: `\e[m0;34`
3. Define console text: `\e[m0;34\u`
4. Stop color scheme `\e[m0;34\u\e[m`
5. Add next color
~~~
\e[m0;34\u\e[m \e[m0;36\h\e[m`
         ^username      ^hostname
          color:          color:
          0;34            0;36
~~~

Example:

PS1=’\e[0;30

~~~
Colors:
| Color  | Value|        
|--------+------|
| Black  | 0;30 |
|--------+------|
| Blue   | 0;34 |
|--------+------|
| Green  | 0;32 |
|--------+------|
| Cyan   | 0;36 |
|--------+------|
| Red    | 0;31 |
|--------+------|
| Purple | 0;35 |
|--------+------|
| Brown  | 0;33 |
|--------+------|
| Blue   | 0;34 |
|--------+------|
| Green  | 0;32 |
|--------+------|
| Cyan   | 0;36 |
|--------+------|
| Red    | 0;31 |
|--------+------|
| Purple | 0;35 |
|--------+------|
| Brown  | 0;33 |
|--------+------|
~~~

## pushd & popd
A useful command.
`pushd /var/log`  → move into _/var/log/_ and add the **current directory** to memory
popd → move out of _/var/log/_ into the previous directory

Example:
~~~bash
matt:/etc$ pushd /home/matt  # I'm in /etc/
matt:/home/matt$ popd        # I'm in /home/matt
matt:/etc$                   # Back in /etc/
~~~

## trap
The `trap` command will run one or more commands when the shell gets a **signal**,
usualy from the `kill` command. 


Example: 
~~~bash
trap ‘echo trapped, baby!’ 5
 #    ^ command to execute  ^ once this signal is present
~~~      

To execute the trap:
~~~bash
kill -5 $$
#        ^ current PID
~~~

~~~bash
trap ‘echo trapped,baby!’ 5
kill -5 $$
=> trapped,baby!
~~~

The `kill` command sends a signal to the `trap` slot, which causes the echo command to execute.

Note that the signal has to have the correct number. Trap, in this case is waiting for signal *5*,
and we provide it with 

~~~
kill -5  $$
     ^   ^ current process
     sig num
~~~     



## Terminal
### Single command terminal
To run a terminal that executed a single command and quits on command termination:
xterm -e cmd
lxterminal -e cmd

Once cmd is terminated the terminal closes.

## X environment
### Keys in X

~~~
   +-----------+
   | keyboard  |    < physical keyboard
   +-----------+
         |
         ↓ 
   +-----------+
   |  keycode  |    < literal indexes for each key                  ↓ view code numbers using “xev”
   +-----------+    < every key has a code number assigned to it (key 1 has code 2, key “g” has code 34)
         |
         | (translation)
         | 
         ↓ 
   +-----------+
   |  keysym   |    < keycodes after getting translated into human-friendly, standarized names
   +-----------+    < (a = a), (left shift = SHIFT_L), (Left Windows = Super_L). View keysyms using
                                                                                    “xev”
~~~                                                                                    

You can change keysyms using “xmodmap -e”:  
~~~bash
xmodmap -e “keysym BackSpace = Delete”
   #                  ^ assign Delete to BackSpace
~~~                      

Disable a key:  
~~~bash
xmodmap -e “keysym Caps_Lock = “
#                             ^ empty value
~~~                                

Be careful with “showkey”, as it can show incorrect keycodes. Use `xev`.

You can modify the default system keymaps. Install `console-data` → modify files in `/usr/share/keymaps/`.

### Customizing the look of an X application
It’s done using `~/.Xresources` or `~/.Xdefaults` (they are interchangeable), or
`xrdb --merge FILE` (loads config into X)

The settings looks like this:
~~~
xterm.scrollBar:      False   ← variable
↑    ^ dot or asterisk(*)
name of client
~~~
~~~
nameOfClient.variableName:     variableValue
~~~

Comments are marked with exclamation points (!) at the beginning of the line.

~~~
this.is:      Code
! This is a comment
~~~

You can name an instance of an application to have special looks:  
`xterm -name blackxterm`  
This will run xterm with the .Xresources settings of “blackxterm”, as defined:
~~~cfg
blackxterm.background:    black
blackxterm.foreground:    white
~~~
`xterm -name redxterm`
~~~
redxterm.background:    red
redxterm.foreground:    white
~~~

### xrdb
xrdb is a program that saves you from maintaining multiple settings files
(for example if you’re running a server)

`xrdb -load FILE`     < load file into settings memory

`xrdb -query`         < check current settings memory

If two parameters are in conflict, the latter one overrides the former:
xrdb -query
> Xcursor.size:    18
xrdb -load cursorSize25
xrdb -query
> Xcursor.size:    25
                   ^ override

### Event Translations
An event translation is when multiple events (keystrokes, mouse clicks) signify one action.
The window manager recognizes multiple keystrokes and *translates* them into an action.

An example of such action would be selecting text, which consists of three events:
1. Pressing Pointer_Button1
2. Moving the pointer while holding Pointer_Button1
3. Releasing Pointer_Button1

These three events get translated into a SELECTION action.

This event goes like this:
~~~
1. <Btn1Down>: select-start( )\n\
2. <Btn1Motion>: select-extend( )\n
3. <Btn1Up>: select-end(primary,CUT_BUFFER0)
~~~
Events like this are defined in a translation table, where each event
is enclosed in angle brackets (<>), and produces an action that is defined after the colon.

~~~bash
<Event>: action\n
<Event>: function( )\n
                    ^ the translation table is a continuous string, so you need to put
                      newline characters
~~~                      



# Sysadmin
## Processes
What is a process? A process is a running program.
It’s managed by the kernel. 

Every process is a descendant of process 1, which is the initialization
process.  
Process 1 gets forked and that’s how new processes get created.

To find a process:
`pgrep -lf (processName)`

The metadata of a process (maintained by kernel):  
• address ( where is the memory of the process)  
• current state of the process (is it running?)  
• execution priority   
• file usage and network usage  
• owner of the process  

Each process has a Process ID(`PID`) and a User ID(`UID`) associated with it.

• fork()  
   ◦ new processes are cloned from existing processes  
   ◦ original process is referred to as the parent  
   ◦ `PPID` = Parent PID  
   ◦ `UID` stays the same  


### Process states
• Running  
• Sleeping - waiting for resource (disk, network, RAM etc.)  
• Zombie - a child process has terminated execution, and the parent process hasn’t
  garbage collected the child yet  
• Stopped - the kernel has stopped the process  

### Signals
Signals are means of communication between processes.  
• Sent by terminals to take some action  
• Sent by admins  
• Sent by kernel for intervention  
• Sent by kernel for notification  

Most signals can be ignored by the program.

Common signals:

~~~
|----+------+-----------------------------------------------------------|
| #  | Name | Description                                               |
|----+------+-----------------------------------------------------------|
| 1  | HUP  | Hangup - the controlling terminal (tty or pty) has exited |
|----+------+-------------------------+---------------------------------|
| 2  | INT  | Interrupt (usually CTRL + C )                             |
|----+------+-----------------------------------------------------------|
| 3  | QUIT | User requested core dump                                  |
|----+------+-----------------------------------------------------------|
| 9  | KILL | Terminate process immediately                             |
|----+------+-----------------------------------------------------------|
| 11 | SEGV | Segmentation fault (bug)                                  |
|----+------+-----------------------------------------------------------|
| 15 | TERM | Requests termination from process                         |
|----+------+-----------------------------------------------------------|
~~~


Ctrl + z - stop process  
fg - bring back to the front

### Troubleshooting a process
strace - track a process low level  

### cron
Cron executes a program in the background on a specified point in time.  

cron is consisted of crontabs - cron configs  
Format of a crontab:  
~~~
minute hour dayOfMonth month weekday `command`
~~~

example:
~~~
30 16 29 6 2 echo “Hello world”
~~~

You can use `*` to signify any. Example `*` in place of month will execute every month.

Example 2:
~~~bash
*/15 * * * * uptime >> ~/.uptimes
# Appends uptime output to ~/.uptimes, every 15 minutes
~~~


Common uses:  
• Backup  
• Email notification  
• System monitoring ( + send email if something is wrong )  
• Filesystem Cleanup (rotate log files)  
• Restarting services  
• Database maintenance  

## Filesystem
Filesystem composition:  
• a namespace organized as a hierarchy  
• Application Programming Interface (API) → enables us to run programs  
• security model to protect and share objects (permissions for files)  
• interface between hardware and software  

The kernel supports many filesystem types.   
The Linux filesystem is usually ext3, or ext4. There are many types
of filesystems that support various features. 

Purposes:  
• represent and organize different storage systems (CD/DVD/usb disk)  
• interprocess communication (through filesystem abstractions)  
• organize and store files for the system  

`pwd` - print working directory

FHS - filesystem hierarchy standard - how UNIX filesystems should be organized

### Filesystem composition
`/bin` - executables  
`/boot` - location of the kernel, loaded by bootloaded,  
        commonly separate filesystem on the disk  
`/cgroup` - kernel abstraction, not a standard        
`/dev` - location of devices  
`/home`  
`/lib` - library locations  
`/lib64`  
`/media` - removable media  
`/mnt` - temporary mounts  
`/opt` - optional software  
`/proc` - pseudo filesystem that exposes kernel config and process config  
`/root` - special dir for root user (home dir)  
`/sbin` - sensitive binary files that only root can use  
`/tmp` - storing temporary files (you can use it too)  
         In Ubuntu /tmp is removed on reboot  
`/usr` - shared user data  
`/var` - variable data files, sometimes databases put files here  
       the point of this is that the files here are constantly  
       changing
### Filesystem permissions
Permissions:  
• `r` - read  
• `w` - write  
• `x` - execute  

For directories, execute allows scanning. If directory has no
x permission, you can’t list it or run commands on it.

Permission are represented in octal format.
And they go as follows:
1. User that created the file
2. Group that the user is in
3. Other people

~~~
rwx rx r  ← read (other people)
^   ^ read execute (group)
read write execute (owner)
~~~

Octal representation:  
• read - 4  
• write - 2  
• execute - 1  

To set a permission on a file:
~~~
chmod 0(octal)(octal)(octal) file
~~~

~~~
chmod 0777 file
        ^
        everybody can read, write, and execute

chmod 0764 file
       ^ owner can rwx,
         group can rw,
         other users can r
~~~         


getfacl & setfacl:

To view permissions of a file in detail:  
`getfacl (file)`  
To give special permissions to a *specific* user:  
`setfacl -m user:matt:rwx`

## Syslog:
Unified approach to capturing log files.  
Syslog can sort files by facility and severity.  

Sorting by facility - where the log came from  
Sorting by severity - how important is the log  

Ubuntu uses `rsyslog`, as the original `syslog` is outdated.

Sample facilities:  
0 - kernel  
1 - user level  
2 - mail system  

So if the message comes from facility “0”, it’s a kernel message. “1” - user-level message.

Severity sorting:
~~~
0 - emerg - system is unusable
1 - alert - action must be taken immediately
2 - crit - mail system
3 - err - system daemon
4 - warn - system and authentication
5 - notice - normal but significant condition
6 - info - information
7 - debug - verbose debug info
~~~

Ordinary logs are stored in `/var/log/messages`

You can use the `logger` command to send a log into a syslog file 
Example:  
~~~
logger -p info “Hello world”
           ^ priority “info” (severity 6)
~~~         

~~~
logger -p local1`.`emerg “im taking a break”
           ^      ^ `severity`
           `facility`
~~~           

You can check how the logfiles are sorted in `/etc/rsyslog.conf`

It’s important to store your logfiles on a separate system,  
as that protects your logs from being modified in case the system gets hacked.  

Therefore, you send your logs over a network to another system.  
You configure that in `/etc/rsyslog.conf`

Instead of a file location, send to:  
~~~
@@ 192.168.xx.xx (local IP) → sends over TCP
~~~

`SEC` - Simple Event Correlator:  
If there are logs that match regex, do something.  
Example: you’re getting 100 login attempts from one IP address → ban IP
