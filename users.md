# Bletchley Users Guide

## Table of Contents

* [The Bletchley Cluster](#the-bletchley-cluster)
* [Accessing Bletchley](#accessing-bletchley)
* [Command Line Basics](#command-line-basics)
* [The Slurm Workload Manager](#the-slurm-workload-manager)
* [Language-Specific Examples](#language-specific-examples)
    * [Docker](#docker)
    * [Gaussian](#gaussian)
    * [Matlab](#matlab)
    * [Python](#python)
    * [R](#r)
* [Additional Support](#additional-support)
* [Appendix: Bletchley Technical Details](#appendix-bletchley-technical-details)

## The Bletchley Cluster

*Summary: Bletchley is a powerful computer open for use by anyone in the Oxy community.*

Bletchley is a *high-performance computing (HPC) cluster* - a (network of) computers that work together to perform computations. Bletchley is Oxy's own cluster, the result of an [NSF MRI grant](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1919771). In a lot of social and physical sciences, research increasingly involves processing large amounts of data or finding the optimal configuration out of many possible parameters. Doing this requires not only requires lots of computing power, but also multi-day run-times that Oxy's other infrastructure cannot support. The Bletchley cluster exists to support these kinds of research, and also acts as an instrument where students can learn how to apply computational methods.

Bletchley is open to be used by Oxy professors and students for teaching and research. This document serves as a technical user guide to Bletchley, and will walk through how to get access, some technical background on the command line and the workload manager, as well as starter code for running your program on the cluster.

<!--
* what is it? why?
* who can use it?
* who to contact to start using it?
-->

## Accessing Bletchley

*Summary: Contact the HPC committee for a user account, then VPN to Oxy and SSH into `bletchley.oxy.edu`.*

<!-- FIXME should we create an email address for the HPC committee? -->
Bletchley is administered by the HPC committee, which currently includes Profs. [Jeff Cannon](mailto:jcannon@oxy.edu) (Chemistry), [Justin Li](mailto:justinnhli@oxy.edu) (Cognitive Science, Computer Science), [Diana Ngo](mailto:dngo@oxy.edu) (Economics), [Janet Scheel](mailto:jscheel@oxy.edu) (Physics), and [Amanda Zellmer](mailto:zellmer@oxy.edu) (Biology), led by [David Dellinger](mailto:ddellinger@oxy.edu) (ITS). To get access to Bletchley, you should contact one of us with a description of what you plan to use Bletchley for. We would be happy to talk you through Bletchley's capabilities, and how it can best assist with your research or project.

Once we have confirmed that Bletchley is able to support your work, we will create a user account for you on the cluster and email you with your username and password. Since Bletchley hosts research data from many Oxy faculty, including potentially sensitive personal information, it is only accessible from Oxy's network. To log in from off campus, you will need to install a *virtual private network* (*VPN*). A VPN creates a "tunnel" between your computer and Oxy, so that your computer will essentially *be* part of Oxy's network for all intents and purposes. You can use a VPN to access journals that Oxy has subscriptions to, but more importantly for us, you will also be able to log into Bletchley.

__To install a VPN client__, follow [ITS' instructions for installing Fortinet](https://helpdesk.oxycreates.org/kb/?s=VPN) on your [Windows](https://helpdesk.oxycreates.org/kb/2016/07/setting-up-the-windows-fortinet-vpn-client/) or [Mac](https://helpdesk.oxycreates.org/kb/2016/07/setting-up-the-mac-os-x-fortinet-vpn-client/) computer. Once you have installed Fortinet, you should make sure that the connection is activated as well. If you are unable to connect, you may need permission from ITS to use the VPN; please email ITS or the HPC Committee for help.

Be aware that when you are connected to Oxy's VPN, *all* your internet traffic is sent to Oxy first. If you are not comfortable with this, be sure to *only* connect your VPN when needed, and to disconnect when you are done.

Once you have activated the connection to Oxy's network, you can now SSH to log into Bletchley. *SSH* stands for "secure shell", and allows you interact with Bletchley and tell it what to do.

__To SSH into Bletchley from your computer__, you will need a terminal. If you are on a Mac, you can open the Terminal app. On Windows, you can FIXME. Regardless, once you are in the terminal, you should type (replacing `username` with your given username):

```
ssh username@bletchley.oxy.edu
```

Then press Enter. You will then be prompted for password. *Note that when you type your password, no characters will show up - this is normal!* If your log in to Bletchley is successful, your screen should not look markedly different. Here is what my screen shows:

```
[justinnhli@air2019 ~]$ ssh justinnhli@bletchley.oxy.edu
justinnhli@bletchley.oxy.edu's password:
Last login: Sat May  1 15:52:20 2021

[justinnhli@bletchley ~]$
```

To confirm that you've logged in, you can type:

```
hostname
```

This command will print out which computer you are currently on. After you press Enter, the screen print `bletchley.oxy.edu`, which means that you have successfully logged into Bletchley.


<!--
* user accounts (who to contact)
* VPN and SSH
-->

## Command Line Basics

*Summary: Know what "command", "options", and "arguments" are, and know how to use `cd`, `ls`, and `man`. Use a tutorial for everything else.*

The *command line interface* (CLI) or sometimes the *terminal* is a text-based interface for doing things on a computer. CLIs were originally the only way to get a computer to do things, until the development of the graphical user interface (GUI) in the mid 1970s, which eventually led to Windows and OS X that you're familiar with. Here is an example command line session:

```
[justinnhli@surnia-envy ~]$ ls
 bin   courses   Desktop   Downloads   Dropbox   git   Library   media  'My Games'   oxy   papers   pim   scholarship   work
[justinnhli@surnia-envy ~]$ ls -l
total 84
lrwxrwxrwx  1 justinnhli justinnhli    20 2016-05-21 16:15  bin -> git/dotfiles/bin/bin
lrwxrwxrwx  1 justinnhli justinnhli    18 2021-07-07 17:53  courses -> oxy/courses/202201
drwxr-xr-x  4 justinnhli justinnhli  4096 2021-07-07 11:10  Desktop
drwx------  4 justinnhli justinnhli 32768 2021-07-17 11:50  Downloads
drwx------ 14 justinnhli justinnhli 12288 2021-07-19 17:21  Dropbox
drwxr-xr-x 20 justinnhli justinnhli  4096 2021-07-18 21:51  git
lrwxrwxrwx  1 justinnhli justinnhli    24 2020-01-26 22:06  Library -> git/dotfiles/pip/Library
drwxr-xr-x  9 justinnhli justinnhli  4096 2021-07-03 17:31  media
drwxr-xr-x  3 justinnhli justinnhli  4096 2018-05-29 08:33 'My Games'
lrwxrwxrwx  1 justinnhli justinnhli    12 2017-08-15 13:08  oxy -> Dropbox/oxy/
drwxr-xr-x 28 justinnhli justinnhli  4096 2021-01-07 13:32  papers
lrwxrwxrwx  1 justinnhli justinnhli    11 2020-05-19 11:34  pim -> Dropbox/pim
lrwxrwxrwx  1 justinnhli justinnhli    20 2017-01-17 12:04  scholarship -> Dropbox/scholarship/
drwxr-xr-x 11 justinnhli justinnhli  4096 2016-05-25 11:50  work
[justinnhli@surnia-envy ~]$ ls -l Dropbox
total 33516
-rw-r--r--  1 justinnhli justinnhli       51 2017-03-31 18:55  api_keys
drwxr-xr-x  2 justinnhli justinnhli     4096 2021-06-17 15:39  bin
drwxr-xr-x  2 justinnhli justinnhli     4096 2021-07-13 14:02 'Camera Uploads'
drwxr-xr-x  2 justinnhli justinnhli     4096 2021-07-03 12:05  croncheck
drwxr-xr-x  5 justinnhli justinnhli     4096 2020-12-04 23:42  music
drwxr-xr-x  3 justinnhli justinnhli    16384 2021-06-23 11:07  obsidian
drwxr-xr-x 16 justinnhli justinnhli     4096 2020-04-09 21:46  oxy
drwxr-xr-x  8 justinnhli justinnhli     4096 2021-01-10 17:41  personal
drwxr-xr-x  8 justinnhli justinnhli     4096 2021-07-16 19:47  pim
drwxr-xr-x 20 justinnhli justinnhli     4096 2021-07-14 16:08  projects
drwxr-xr-x  5 justinnhli justinnhli     4096 2021-07-06 21:18  reference
-rw-r--r--  1 justinnhli justinnhli   305937 2020-01-22 13:31  scanned-pages.pdf
drwxr-xr-x 10 justinnhli justinnhli     4096 2018-09-07 22:39  scholarship
-rw-r--r--  1 justinnhli justinnhli  2460602 2021-04-17 23:27  wallpaper.png
[justinnhli@surnia-envy ~]$
```

There is a lot of information here that is irrelevant for using Bletchley, so we will focus on four things: the *prompt*, *commands*, *options*, and *arguments*. For example, the first line above (`[justinnhli@surnia-envy ~]$ ls`) has two parts:

* Everything before the dollar sign (`[justinnhli@surnia-envy ~]$`) is the *prompt*. Your specific prompt may be different, but this is the computer waiting for you (or *prompting* you) to tell it to do something.

* The remainder of that line (`ls`) is the *command*. Every command tells the computer to do something - in this case, `ls` asks the computer to *l*i*s*t the files in the current folder. The result of that command is the second line of the transcript above, so you can see that the folder has a `bin` file/folder, a `courses` file/folder, and so on.

In the next line of the transcript, we issued another command (`ls -l`). Like before, this is asking the computer to list files, except we also gave it the `-l` *option*. As the name suggests, this tells the computer to do something slightly different - in this case, don't just list the file names, but also show additional information those files, such as when it was last modified.

Finally, the last command we gave in the transcript is (`ls -l Dropbox`). The final part here (`Dropbox`) is the *argument*, which tells the computer *which folder* to list the contents of. In general, *options* will being with a hyphen (`-`), while *arguments* will not.

To sum up, to use the command line, the computer will give you a *prompt* for you to tell it what to do. Everything you type in is a *command*, and you can specify *how* that command works with *options*, and *what* it runs on with *arguments*.

That's really it! The rest of using the command line is about knowing what commands exist. We will look at a few below.

### Basic Navigation

When you first log into Bletchley, you will be in your "home directory". ("Directory" is another word for "folder".) All your files and folders will be inside this directory, and it's your private workspace on the cluster. To confirm this, the first command is `pwd`:

```
[justinnhli@surnia-envy ~]$ pwd
/home/justinnhli
[justinnhli@surnia-envy ~]$
```

`pwd` stands for "present working directory", and it tells you which folder you are in; in this case, we are currently in `/home/justinnhli`. This means that you are currently in the `justinnhli` folder inside the `home` folder, which is where all home directories exist.

To make things more interesting, let's create a new folder. You can do this with the `mkdir` command:

```
[justinnhli@surnia-envy ~]$ mkdir test
[justinnhli@surnia-envy ~]$
```

`mkdir` stands for "make directory", and the command should have created a folder called `test`. Notice that nothing else was printed out after the `mkdir` command. This is perfectly normal - CLIs tend to be rather terse, so no printout usually means that the command succeeded. To confirm that we did in fact create the `test` folder, we can use `ls`:

```
[justinnhli@surnia-envy ~]$ ls
 test
[justinnhli@surnia-envy ~]$
```

The last command we will talk about here is `cd`, which stands for "change directory". As you might imagine, this will take you inside a folder:

```
[justinnhli@surnia-envy ~]$ cd test
[justinnhli@surnia-envy test]$ pwd
/home/justinnhli/test
[justinnhli@surnia-envy test]$ ls
[justinnhli@surnia-envy test]$
```

The three commands we used here first took us into the `test` folder (`cd test`), then printed out where we were (`pwd`), and finally listed the (currently empty) `test` folder (`ls`). You will notice that the prompt has also changed to show me which folder I am in; I configured my prompt do to this, so don't be worried if yours doesn't.

Finally, we can go back to our home directory using `cd` without any arguments:

```
[justinnhli@surnia-envy test]$ cd
[justinnhli@surnia-envy ~]$ pwd
/home/justinnhli
[justinnhli@surnia-envy ~]$
```

You will use these three commands - `cd`, `ls`, and `pwd` - over and over again as you work on Bletchley, so you should get pretty familiar with what they do and how they work.

### Additional Resources

Mastering the command line takes years, so we recommend learning commands as you need them. If you want to learn more, however, here are some resources you might use:

* [Command Line for Beginners tutorial](https://ubuntu.com/tutorials/command-line-for-beginners) - a complete tutorial on learning the command line.
* [Command Line Cheat Sheet](https://justinnhli.oxycreates.org/bash/) - a handy reference for common commands that you will use.
* [A-Z Index of Commands](https://ss64.com/bash/) - a comprehensive reference for commands you might see.

The last thing we will say is this: Google is your friend. Thousands of people have learned to use the command line and have asked questions on forums like Stack Overflow. If in doubt, just google a command (eg. "ls command line"), and there will be pages and pages of resources to help you figure things out.

<!--
* the command line
* basic navigation
* additional resources
-->

## Uploading Files

* FTP?

## The Slurm Workload Manager

* overall architecture/components
    * queues
* job submission commands
* job monitoring commands

## Language-Specific Examples

### Docker

### Gaussian

### Matlab

### Python

### R

## Additional Support

* who to email

## Appendix: Bletchley Technical Details

As of summer 2021, the cluster is made of 27 individual computers (*nodes*), including:

* 1 *head node* that manages the jobs to be run by the other nodes
* 24 *compute nodes* that does the actual work. Each compute node has:
    * 36 * 2.6GHz CPUs
    * 8 nodes with 2.6, 10.6, and 21.3 GBs RAM/CPU each
    * 960 GB hard disk space
* 1 *GPU node* with several GPUs to speed up certain kinds of operations
* 1 *storage node* that can store results and datasets
    * 2 * 960GB hard disk space
    * 34 * 8TB long-term storage space

<!--
* what is available?
    * in terms of computing power and storage
    * in terms of hardware
-->
