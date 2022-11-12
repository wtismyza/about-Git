# 2. Git Basics

Getting a Git Repository
--------------------------
You typically obtain a Git repository in one of two ways:
1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2. You can clone an existing Git repository from elsewhere.

If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few git add commands that specify the files you want to track, followed by a git commit:

```
$ git init
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```
If you want to clone the Git linkable library, you can specify the new directory name as an additional argument:
```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
Recording Changes to the Repository
-----------------------------------
Checking the Status of Your Files: `git status`
(GitHub changed the default branch name from master to main in mid-2020, and other Git hosts followed suit. However, Git itself still uses master as the default, so we will use it throughout the book.)
Tracking New Files: `git add README`

For convenience, you can try `git add -u` or `git add -A`

Ignoring Files
---------------
You can create a file listing patterns to match them named `.gitignore`.

The rules for the patterns you can put in the .gitignore file are as follows:
1. Blank lines or lines starting with # are ignored.
2. Standard glob patterns work, and will be applied recursively throughout the entire working tree.
3. You can start patterns with a forward slash (/) to avoid recursivity.
4. You can end patterns with a forward slash (/) to specify a directory.
5. You can negate a pattern by starting it with an exclamation point (!).

Viewing Your Staged and Unstaged Changes
----------------------------------------
To see what you’ve changed but not yet staged, type `git diff` with no other arguments.
If you want to see what you’ve staged that will go into your next commit, you can use `git diff --staged` (or git diff --cached). This command compares your staged changes to your last commit.


Committing Your Changes
-----------------------
The simplest way to commit is to type `git commit`.
Alternatively, you can type your commit message inline with the commit command by specifying it after a -m flag: `git commit -m "commit message hahaha"`

You can also skip the staging area: adding the -a option to the git commit command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the git add part.


Removing Files
--------------
run git rm, it stages the file’s removal.

Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the `--cached` option:

```$ git rm --cached README```

You can pass files, directories, and file-glob patterns to the git rm command. That means you can do things such as:

```$ git rm log/\*.log```
Note the backslash (`\`) in front of the `*`. This is necessary because Git does its own filename expansion in addition to your shell’s filename expansion. This command removes all files that have the `.log` extension in the `log/` directory. Or, you can do something like this:

```$ git rm \*~```
This command removes all files whose names end with a `~`.

Moving Files
------------

Unlike many other VCSs, Git doesn’t explicitly track file movement. If you rename a file in Git, no metadata is stored in Git that tells it you renamed the file. However, Git is pretty smart about figuring that out after the fact.

If you want to rename a file in Git, you can run something like:
```$ git mv file_from file_to```

However, this is equivalent to running something like this:
```
$ mv README.md README
$ git rm README.md
$ git add README
```

Viewing the Commit History
--------------------------
Run `git log` command, to see what has happened.

One of the more helpful options is `-p` or `--patch`, which shows the difference (the patch output) introduced in each commit. You can also limit the number of log entries displayed, such as using`-2` to show only the last two entries.

If you want to see some abbreviated stats for each commit, you can use the `--stat` option.

Another really useful option is --pretty. This option changes the log output to formats other than the default. A few prebuilt option values are available for you to use. The oneline value for this option prints each commit on a single line, which is useful if you’re looking at a lot of commits.

**A really good command line:**
```git log --graph --pretty=oneline --abbrev-commit```

Preventing the display of merge commits: Depending on the workflow used in your repository, it’s possible that a sizable percentage of the commits in your log history are just merge commits, which typically aren’t very informative. To prevent the display of merge commits cluttering up your log history, simply add the log option `--no-merges`.

Undoing Things
---------------
One of the common undos takes place when you commit too early and possibly forget to add some files, or you mess up your commit message. If you want to redo that commit, make the additional changes you forgot, stage them, and commit again using the `--amend` option:

```$ git commit --amend```
This command takes your staging area and uses it for the commit. If you’ve made no changes since your last commit (for instance, you run this command immediately after your previous commit), then your snapshot will look exactly the same, and all you’ll change is your commit message.

**Unstaging a Staged File**: `git reset HEAD <file>`

**Unmodifying a Modified File**: `git checkout -- <file>`


