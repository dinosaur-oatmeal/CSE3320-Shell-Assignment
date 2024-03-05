# Unix Shell

In this project, I built a simple Unix shell. The shell is the heart of
the command-line interface, and thus is central to the Unix/C programming
environment. Mastering use of the shell is necessary to become proficient in
this world; knowing how the shell itself is built was the focus of this
project.

There are three specific objectives to this assignment:

* To further familiarize myself with the Linux programming environment.
* To learn how processes are created, destroyed, and managed.
* To gain exposure to the necessary functionality in shells.

## Overview

In this assignment, I implemented a *command line interpreter (CLI)* or,
as it is more commonly known, a *shell*. The shell should operate in this
basic way: when I type in a command (in response to its prompt), the shell
creates a child process that executes the command I entered and then prompts
for more user input when it has finished.

The shell I implemented is similar to, but simpler than, the one I run
every day in Unix.

### Source Code Directories
msh - contains the file msh.c, which I modified for this assignment.

tester - test harness. None of these files were modified by me
(made by the class instructor to test the completed msh.c file)

### Building the Shell
The shell can be built with:
```
make
```
### Testing the Shell
The shell can be ran with:
```
./msh
```

### Basic Shell: `msh`

My basic shell, called `msh` is an interactive loop: it repeatedly 
prints a prompt `msh> ` (note the space after the greater-than sign), parses the 
input, executes the command specified on that line of input, and waits for 
the command to finish. This is repeated until the user types `exit`.

The shell can be invoked with either no arguments or a single argument;
anything else is an error. Here is the no-argument way:

```
prompt> ./msh
msh> 
```

At this point, `msh` is running, and ready to accept commands. Type away!

The mode above is called *interactive* mode, and allows the user to type
commands directly. The shell also supports a *batch mode*, which instead reads
input from a file and executes commands from therein. Here is how you
run the shell with a batch file named `batch.txt`:

```
prompt> ./msh batch.txt
```

One difference between batch and interactive modes: in interactive mode, a
prompt is printed (`msh> `). In batch mode, no prompt is printed.

I structured my shell such that it creates a process for each new
command (the exception are *built-in commands*, discussed below).  My basic
shell is able to parse a command and run the program corresponding to
the command.  For example, if the user types `ls -la /tmp`, my shell
runs the program `/bin/ls` with the given arguments `-la` and `/tmp` 

## Structure

### Built-in Commands

Whenever my shell accepts a command, it should check whether the command is
a **built-in command** or not. If it is, it should not be executed like other
programs. Instead, my shell invokes my implementation of the built-in
command.

In this project, I implemented `exit` and `cd` as built-in
commands.

* `exit`: When the user types `exit`, my shell calls the `exit`
  system call with 0 as a parameter. It is an error to pass any arguments to
  `exit`. 

* `cd`: `cd` always take one argument (0 or >1 args is signaled as an
error). To change directories, I used the `chdir()` system call with the argument
supplied by the user; if `chdir` fails, the program returns an error.

### Redirection

Many times, a shell user prefers to send the output of a program to a file
rather than to the screen. Usually, a shell provides this nice feature with
the `>` character. Formally this is named as redirection of standard
output. To make my shell users happy, my shell includes this
feature, but with a slight twist (explained below).

For example, if a user types `ls -la /tmp > output`, nothing should be printed
on the screen. Instead, the standard output of the `ls` program should be
rerouted to the file `output`. In addition, the standard error output of
the program should be rerouted to the file `output`.

If the `output` file exists before I run my program, I simply
overwrite it (after truncating it).  

The exact format of redirection is a command (and possibly some arguments)
followed by the redirection symbol followed by a filename. Multiple
redirection operators or multiple files to the right of the redirection sign
are errors.
