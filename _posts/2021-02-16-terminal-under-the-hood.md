---
title:  "Terminal under the hood - TTY & PTY"
last_modified_at: 2021-02-17T23:20:25-04:00
classes: wide
author_profile: true
author: yakout-linux
toc: true
categories:
  - blog
tags:
  - [linux, tty, unix]
excerpt: ""
header:
  overlay_image: /assets/tty/tty_unix.png
---

# Introduction

I found that I spend almost half of my day in the terminal, it has been my second home, so I try to understand every thing in it and how it works, every trick and tip I can do with it and install some cool and useful CLI tools out there to use them in my daily programming life to make it easier. So it was important for me to understand how it works.

I did some research in this area and I wrote this blog post that talks about very important definitions in the terminal word like TTY and PTY which are very important terms that every developer and Linux advanced user should know.


# History

To avoid being boring, I won't say much in this section I just want to give you a nice and important overview about TTY that will help you understand it's evolution. If you don't like history you can jump to the next section.

`tty` is an abbreviation for `Teletype` which is basically a machine that sends the keystrokes you typed on the keyboard through a physical cable that is connected to another physical machine that will interpret these keystrokes encoded in electric signals and prints them on a real paper.

<figure><a href="/assets/tty/tty_brand.png"><img src="/assets/tty/tty_brand.png"></a></figure>

When computers were invented, engineers decided to reuse those teletypes to communicate to them instead of inventing new devices specially for them.

<figure><a href="/assets/tty/tty_old_2.png"><img src="/assets/tty/tty_old_2.png"></a></figure>

The IBM 7040. IBM Selectric-typewriter-based terminals appear to the left and right, probably locally attached to the 7040 as consoles. [source](http://www.columbia.edu/cu/computinghistory/1965.html)

Here is another clear photo of IBM Selectric-typewriter ([source](https://www.ibm.com/ibm/history/ibm100/images/icp/Z491903Y91074L07/us__en_us__ibm100__selectric__selectric_2__900x746.jpg))

<figure><a href="/assets/tty/tty_old_1.png"><img src="/assets/tty/tty_old_1.png"></a></figure>

When talking about TTY I love to show this awesome picture (Blog post header photo). It really shows a lot of things!

<figure><a href="/assets/tty/tty_unix.png"><img src="/assets/tty/tty_unix.png"></a></figure>

In this picture you can find Ken Thompson (co-invented Golang) and Dennis Ritchie (created C programming language) both were working on developing the first version of Unix.
In front of them, you can see the 16-bit minicomputers [PDP-11](https://en.wikipedia.org/wiki/PDP-11) and Two Teletype model [ASR 33](https://en.wikipedia.org/wiki/Teletype_Model_33) terminals, one of them Ken is typing on. ([source](http://www.stargatemuseum.org/html/dennis_ritchie__ken_thompson__.html))

- In the 1970s there were two major types of computer terminals available, Teletype 33 and IBM Selectric Terminal.

    <figure><a href="/assets/tty/tty_original.png"><img src="/assets/tty/tty_original.png"></a></figure>

    Teletype 33

ASR 33 machines were predominantly used as hardcopy terminals for early computers before electronic terminals with screen existed.

Later as the technology improved and video displays were introduced, CRT terminals eventually dethroned the Teletypes.

<figure><a href="/assets/tty/crt_tty.png"><img src="/assets/tty/crt_tty.png"></a></figure>

That's it enough history for now let's jump to TTY in our modern computers.

---

# What is TTY

In this section we are going to understand how computers at the end of the cable dealt with issues like seeing what has been typed and erasing with backspace, and how this is related to modern computers terminals.

This is a diagram of how teletypes used to work in early computers

<figure><a href="/assets/tty/old_tty_diagram.png"><img src="/assets/tty/old_tty_diagram.png"></a></figure>

As we see, Teletype is connected through a pair of wires to UART (Universal Asynchronous Receiver and Transmitter). The UART driver manages the physical transmissions of bytes and sends them to the line discipline (LDISC) which is provided by the kernel to allow editing commands features like backspace, erase word, and clear line, it also handles special characters such as the interrupt character (CTRL + C).

Advanced applications like vim and ssh disables these features by putting the line discipline into raw mode, so they can handle all these stuff themselves.

The kernel provides many line disciplines but only one is enabled by default and connected to a serial device. The default one, which provides line editing, is called `N_TTY`. 

The line discipline does not handle keys like Up, Left, Down, Right, that's why when you press them you will see something like `^[A` the `^[` is called the escape sequence. the terminal emulator encodes keystrokes that has no character representation
{: .notice--success}

The TTY driver is also provided by the kernel, it implements session management which will help user to have many processes running at the same time and only interacting with foreground process which his/her input will be redirected to it and only the foreground process will be allowed to send output to the TTY device.

In modern computers, TTY driver and line discipline behave the same, but there is no UART or physical terminal involved anymore.

<figure><a href="/assets/tty/tty_diagram.png"><img src="/assets/tty/tty_diagram.png"></a></figure>

A seat consists of all hardware devices (display, keyboard, mouse ..) assigned to a specific workplace.

💡 In Linux you can list seats using `loginctl list-seats`, and `loginctl seat-status seat0` to list devices assigned to seat `seat0`
{: .notice--info}

💡 There are 7 ttys in the system, to switch to one of them use CTRL + ALT + F[1 - 7].<br />
In ArchLinux F7 will get you back to the GUI while in ubuntu it's F2, you can also use `chvt [tty number]` to switch between ttys in the terminal. (`chvt` stands for change virtual terminal)<br /><br />
Some people use a tty to run commands instead of a terminal emulator in the GUI if they are doing a long running task (e.g system update or moving home directory to another hard disk) and they are afraid the graphical desktop environment (window manager) will crash or freezes causing issues to the task.
{: .notice--success}

**Fun fact:**<br />Terminal emulator is launched from a graphical environment that is itself launched from a virtual terminal.
{: .notice--success}

**Recap:**
- The words terminal and TTY device are used interchangeably.
- Terminal used to be a physical device that sends characters to the TTY driver.
- The kernel in modern computers emulates the physical terminal device.
- A terminal emulator is as dumb as the physical terminals used to be, it listens for events coming from the keyboard and sends it down to the driver. The difference is that there is no physical device or cable which is connected to the TTY driver.
- line discipline is implemented by the TTY driver which is a kernel module.


# What's a pseudo terminal PTY? (PTY vs TTY)

Simply It's Teletype emulated by a computer program running in the user land, in contrast, TTY is a kernel program/emulator.

> The need to move the terminal emulation in user space, while still keeping the TTY subsystem that provides session management and line discipline, led to the development of pseudo terminals.

**Note:** The difference between kernel space and user space is that kernel space has access to the hardware directly while the user space interacts with the kernel only.
{: .notice--success}

PTY will behave like TTY, except that there is no attachment to a seat; you can open several terminal emulator windows at the same time (multiple PTYs) and display them side by side, having different sessions running in parallel.

PTY is pair of master and slave sides, Writing to the master is exactly like typing on a terminal, thus the master pseudo-device acts like the person sitting in front of the physical computer text terminal, whereas the slave pseudo-device emulates a physical computer text terminal, so anything you write in the master will appear in the slave and anything written in the slave will appear to the master.

when you open a terminal emulator program, it forks a process that requests a *pty* pair from the OS and starts a new session which is a group of processes running under control of a single user (i.e shell), it is defined by the first process that attach the pts (e.g bash).

The slave part (pts) is represented by a file in /dev/pts/N where N is a number, you can know the what slave your session is attached to using `tty` or `ps` commands.

The master part (ptm), is not represented on the file system. It is represented by a file descriptor obtained by the system call that creates a pty.

**Note:**
In Linux, a process requests a pty from OS by opening the special device `/dev/ptmx`.
{: .notice--success}

This below diagram shows how your favorite terminal emulator program works.

<figure><a href="/assets/tty/pty_diagram.png"><img src="/assets/tty/pty_diagram.png"></a></figure>

## Shells & Sessions

A shell is what allows you to interact with your computer and OS, when I mention the word `session` , it means shell session which acts as a layer between the kernel and the user, so whenever you run a command in your terminal emulator it's the shell (e.g bash) that listens for them on the PTY slave attached to it, captures them, communicate with the kernel then write the output back to the PTY slave which the TTY driver will see it and send it to the PTY master so it can be shown in the terminal emulator UI.

The process that is running shell is known as session leader. Every other process that is running in the terminal is child of this session leader. These child processes form process groups that are controlled by this session leader (parent process), and when you logout from the terminal, kernel sends SIGHUP (kill -1) to the session leader, that will propagate this signal to each child process.

Each session is managed by a session leader, the shell, which is cooperating tightly with the kernel using a complex protocol of signals and system calls.

# Example: What happens when you type `ls` command in terminal?

1. You open the terminal emulator which requests a `pty` from the OS that will return a pair of file descriptors for the master and slave sides, then starts the bash program as a sub-process and attach it to the slave part.
2. The terminal emulator sends the characters to the master side, the TTY driver takes the character echo them back to the master and buffers them.
3. When you hit enter the TTY driver copies the characters in the buffer to the slave.
4. The bash takes it's input from the slave (std input steam points to pts) so when you type `ls` and press enter it will start a child sub-process, executes the command and writes the output to the std output which also points to the pts.
5. The tty driver will take the output from pts and echo it to the master
6. The terminal emulator program will redraw it's GUI to show you the results of commands you typed.

# Hands on using python

To illustrate how the stdin, stdout, and stderr are attached to the pts allocated by the shell, let's run this python3 code from the terminal:

```python
# pty.py

import sys

print(f'stdin: {sys.stdin.isatty()}')
print(f'stdout: {sys.stdout.isatty()}')
print(f'stderr: {sys.stderr.isatty()}')
```

`isatty` returns True if the file is associated with a terminal device.

```python
$ python3 pty.py
stdin: True
stdout: True
stderr: True
```

so if we run the script using:

```python
$ echo 'hi' | python3 tty.py
stdin: False
stdout: True
stderr: True
```

we can notice that stdin is no more a tty since it takes the input from a pipe.

You can also try:

```python
$ python3 tty.py > output && cat output
stdin: True
stdout: False
stderr: True
```

You can also write to the tty directly try opening a two terminals side by side, then run `tty` on one terminal so you know the path of the `pts` then, on the other terminal run this after replacing `/dev/tty` with the pts path of the other terminal:

```ruby
python3 -c 'print("stdin"); open("/dev/tty", "w").write("tty\n")' &>/dev/null
```

Even though you have redirected your standard streams this will write to the other terminal.
despite it's a bad idea to write to the tty directly as it can mess up the display
the sudo command does that. (This info may help you debug some issues ;) )

**Important note**:<br />
If you run `ls -l /dev` you will find some entries like this
`crw--w---- 1 root tty 4, 0 Apr 17 23:10 /dev/tty0`
The **c** at the beginning means it is a character device. Although these files have the same primitives of regular files (open, read, write, etc.), they are not a representation of data on the disk - that is, they might be weird.
{: .notice--info}


# Useful Resources about the topic

1. [http://www.linusakesson.net/programming/tty/](http://www.linusakesson.net/programming/tty/)
2. [https://www.yabage.me/2016/07/08/tty-under-the-hood/](https://www.yabage.me/2016/07/08/tty-under-the-hood/)
3. [https://dev.to/napicella/linux-terminals-tty-pty-and-shell-192e](https://dev.to/napicella/linux-terminals-tty-pty-and-shell-192e)
4. [https://www.win.tue.nl/~aeb/linux/lk/lk-11.html](https://www.win.tue.nl/~aeb/linux/lk/lk-11.html)
5. [https://www.youtube.com/watch?v=SYwbEcNrcjI](https://www.youtube.com/watch?v=SYwbEcNrcjI)
6. [ASR 33 demo] [https://www.youtube.com/watch?v=S81GyMKH7zw](https://www.youtube.com/watch?v=S81GyMKH7zw)
7. [https://www.freedesktop.org/wiki/Software/systemd/multiseat/](https://www.freedesktop.org/wiki/Software/systemd/multiseat/)
8. [https://man7.org/linux/man-pages/man4/ptmx.4.html](https://man7.org/linux/man-pages/man4/ptmx.4.html)
9. [https://unix.stackexchange.com/questions/117981/what-are-the-responsibilities-of-each-pseudo-terminal-pty-component-software](https://unix.stackexchange.com/questions/117981/what-are-the-responsibilities-of-each-pseudo-terminal-pty-component-software)
10. [https://yannik520.github.io/tty/tty_driver.html](https://yannik520.github.io/tty/tty_driver.html)
11. [https://www.linux.it/~rubini/docs/serial/serial.html](https://www.linux.it/~rubini/docs/serial/serial.html)
12. [https://kb.novaordis.com/index.php/Linux_TTY](https://kb.novaordis.com/index.php/Linux_TTY)
13. [https://en.wikipedia.org/wiki/Devpts](https://en.wikipedia.org/wiki/Devpts)
