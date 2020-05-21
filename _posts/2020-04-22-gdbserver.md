---
layout: post
title: "Using GDB remotely"
tags:
  - english
  - linux
  - gdb
  - degub
---

# Debugging remotely with gdbserver

This year has been an awkward one. All COVID-19 situation has been changing a lot the way we work. Suddenly everyone needs to start to work remotely and learn new tools to do so.
I had been learning a lot of terminal tools and using a minimal graphical interface as much as possible in my daily activities, just for fun. So this transition to remote work was, basically, in my environment, with an ssh connection in the middle.
One of the tools that I've started to use more often was gdbserver.

In this guide, I am going to give you a quite basic tutorial on how to use gdbserver to debug your Linux application.

<!-- more -->

# What is the gdbserver
"Remote debugging" is the process of debugging an application running on a different machine. The developer's system is called the host machine, and the machine running the application is the target.

The gdbserver is a tool to work with the gdb (GNU Debugger tool) remotely. It provides probes and all the necessary tools to debug an application and connect it with external gdb, and by external, I mean in any machine.

So gdbserver only joins with the application and wait for gdb to connect.

# Running gdbserver
The gdbserver receives the commands from a serial line or a network socket. Here I'm going to show how to use it through the network.
Notice: You must compile your application with debug symbols (-g option on compile) to get some useful information, as we did in the [previous post]:(2019-11-16-core_dump.md).

```bash
(target machine)$ gdbserver localhost:1234 app_exec
```

# Connect with gdb
On the gdb, you need the same binary running on the gdbserver and the source code.
On the code folder, run the gdb passing the binary as a parameter.

```bash
(host machine)$ gdb app_exec
```

If your application has a lot of shared libraries, take a look at the [sysroot](https://sourceware.org/gdb/onlinedocs/gdb/Files.html) command to load them.

Now you need to connect the gdb to your server. For this, use the command remote.

```
(inside gdb-host)$ target remote 192.168.1.10:1234
```

With the command above, you set your target as remote passing the IP and port address to it.

# Commands to control the flow of your application

After you connect the gbd to gbdserver, you should be able to control your application. For these, you can use all the features that you are used to, e.g., as program breakpoint, data breakpoints, step, run, backtrace.

help: list command classes

## Basic program flow
* __run__:  run the program with current arguments
* __cont__: continue the program
* __step__: single step at the program; step into functions
* __next__: step by step over functions
* __finish__: finish current function's execution
* __kill__: kill current executing program
* __quit__: quit gdb

## Breakpoint
* __info breakpoints__: show breakpoints
* __delete 1__: delete a breakpoint by its number
* __delete__: delete all breakpoints
* __tbreak \<function\|line\>__: set a temporary breakpoint
* __break \<function\>__: set a breakpoint on a function. Ex.: "break main"
* __break \<number\>__: set a breakpoint on a line number . Ex.: "break 124" 
* __break \<file\>:\<line\>__: set breakpoint at file and line
* __watch expression__: set software watchpoint on variable
* __info watchpoints__: show current watchpoints

## Stack backtrace
* __bt__: print stack backtrace
* __frame__: show current execution position (frame)
* __frame \<number\>__: go to the frame number
* __up__: move up stack trace (towards main)
* __down__: move down stack trace (away from main)

## Inspect
* __info locals__: print automatic variables in frame
* __info args__: print function parameters
* __print expression__: print expression, added to value history
* __ptype name__: print type definition
* __set variable = expression__: assign value

