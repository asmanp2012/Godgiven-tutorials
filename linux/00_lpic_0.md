# Linux Essentials

[Book](https://github.com/nkatre/Free-DevOps-Books-1/blob/master/book/Linux%20Essentials%20-%20Christine%20Bresnahan%20%26%20Richard%20Blum.epub)

## Chapter1: Selecting an Operating System

### What Is the Linux Essentials Certification?

The Linux Professional Institute, or [LPI](www.lpi.org), offers a series of Linux certifications. These certifications aim to provide proof of skill levels for employers; if you’ve passed a particular certification, you should be competent to perform certain tasks on Linux computers.

### Open source

### Linux history

- [Unix history](https://youtu.be/DXh2_CTJW9w)

### What Is an OS?

- Kernel
- Call system
- Shell

### Compare linux with other os

### What Is a Distribution?

- [distrowatch.com](https://distrowatch.com/)

## Chapter 2: Understanding Software Licensing

### Copy Right

### What is License

- [List of all Licenses](https://spdx.org/licenses/)
- [How to chose License](https://choosealicense.com/)

### Open source organizations

- [The Free Software Foundation (FSF)](https://www.fsf.org/)
- [The Open Source Initiative (OSI)](https://opensource.org/)
- [The Creative Commons (CC)](https://creativecommons.org/)

## CHAPTER 3: Investigating Linux’s Principles and Philosophy

### Exploring Linux through the ages

- [Tanenbaum Torvalds debate](https://en.wikipedia.org/wiki/Tanenbaum%E2%80%93Torvalds_debate)
- Using Open Source Software
- Understanding OS Roles: Embedded Computers, Desktop and Laptop Computers, Server Computers

## CHAPTER 4: Using Common Linux Programs

- Using a Linux desktop environment:
  - Gnome
  - Kde
  - Unity
  - Xfce
  - Build Your Own
- Working with productivity software

### Using server programs

- Protocole
- Protocol Layering
- Defult Protocol of services [/etc/services](https://man7.org/linux/man-pages/man5/services.5.html)
- Commands
  - `ls`: List
  - `cd`: Change directory
  - `cat`: Content of one or multi file, concatenate.
  - `pwd`: Print working directory

### Boot linux life cycle

- BIOS
- MBR
- GRUB
- Kernel
- Init (PID=1): the parent of all processes, executed by the kernel during the booting of a system.
  - All program that stand on init process is `System v init`, `SystemD`, `Upstart(Deprecated)`, `OpenRC` and `runit`;
  - `System v init` (Run levels: A runlevel is an operating state on a Unix and Unix-based operating system (`System v init`) that is preset on the Linux-based system.)
    - `Runlevel 0` : shuts down the system
    - `Runlevel 1` : single-user mode
    - `Runlevel 2` : multi-user mode without networking
    - `Runlevel 3` : multi-user mode with networking
    - `Runlevel 4` : user-definable
    - `Runlevel 5` : multi-user mode with networking
    - `Runlevel 6` : reboots the system to restart it
  - `SystemD` (Targets: **At the systemD Target replace with Runlevel**)
    - `poweroff.target`: `Runlevel 0` or shuts down the system
    - `rescue.target`: `Runlevel 1` or single-user mode
    - `multi-user.target`: `Runlevel 2` & `Runlevel 3` & `Runlevel 4` or user-definable & multi-user & networking
    - `graphical.target`: `Runlevel 5` or multi-user mode with networking
    - `reboot.target`: `Runlevel 6` or reboots the system to restart it
  - Where is config of runlevels?
    - System V: `/etc/rc.d/`
    - SystenD: `/etc/systemd/system`
  - How can i manage services?
    - System V [`service`](https://explainshell.com/explain?cmd=service)
      - start [`service nginx start`](https://explainshell.com/explain?cmd=service+nginx+start)
      - stop [`service nginx stop`](https://explainshell.com/explain?cmd=service+nginx+stop)
      - status [`service nginx status`](https://explainshell.com/explain?cmd=service+nginx+status)
      - restart [`service nginx restart`](https://explainshell.com/explain?cmd=service+nginx+restart)
    - SystemD [`systemctl`](https://man7.org/linux/man-pages/man1/systemctl.1.html)
      - enabel
      - disable
      - start
      - stop
      - restart
      - reload
