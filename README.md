# 1. Getting started

What is Git
------------------
As a distributed version control system (DVCS), Git thinks about its data more like **a stream of snapshots**. (unlike **delta-based** version control that keeps information about a set of files and the changes made to each file over time.)

**Question: does Git have memory issues since it keeps snapshots of all the information?** (**more to be answered**)
1. zlib compression 
2. tree structure to keep the same file in different commits. see [Git snapshot](https://www.zhihu.com/question/282558019).

Three States
-------------------
Git has three main states that your files can reside in: **modified**, **staged**, and **committed**.

First-Time Git Setup
--------------------
Git comes with a tool called git config that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories. If you pass the option --system to git config, it reads and writes from this file specifically. Because this is a system configuration file, you would need administrative or superuser privilege to make changes to it.

2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally to you, the user. You can make Git read and write to this file specifically by passing the --global option, and this affects all of the repositories you work with on your system.

3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository. You can force Git to read from and write to this file with the --local option, but that is in fact the default. Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.

Each level overrides values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`.

You can view all of your settings and where they are coming from using:

`$ git config --list --show-origin`

Your Identity
The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

**Try not to use your real email when pushing your code in Github, and use ID + Username @users.noreply.github.com instead.**

Getting Help
------------------
If you ever need help while using Git, there are three equivalent ways to get the comprehensive manual page (manpage) help for any of the Git commands:
```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

# 2. Git Basics



