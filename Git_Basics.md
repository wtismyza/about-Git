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
1. Blank lines or lines starting with `#` are ignored.
2. Standard glob patterns work, and will be applied recursively throughout the entire working tree.
3. You can start patterns with a forward slash (`/`) to avoid recursivity.
4. You can end patterns with a forward slash (`/`) to specify a directory.
5. You can negate a pattern by starting it with an exclamation point (`!`).

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

Showing Your Remotes
--------------------
To see which remote servers you have configured, you can run the `git remote` command. It lists the shortnames of each remote handle you’ve specified. If you’ve cloned your repository, you should at least see origin — that is the default name Git gives to the server you cloned from.

You can also specify `-v`, which shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote.

Adding Remote Repositories
--------------------------
We’ve mentioned and given some demonstrations of how the git clone command implicitly adds the origin remote for you. Here’s how to add a new remote explicitly. To add a new remote Git repository as a shortname you can reference easily, run `git remote add <shortname> <url>`

Fetching and Pulling from Your Remotes
--------------------------------------
As you just saw, to get data from your remote projects, you can run:

```$ git fetch <remote>```
The command goes out to that remote project and pulls down all the data from that remote project that you don’t have yet. After you do this, you should have references to all the branches from that remote, which you can merge in or inspect at any time.

If you clone a repository, the command automatically adds that remote repository under the name “origin”. So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned (or last fetched from) it. It’s important to note that the `git fetch` command only downloads the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.

If your current branch is set up to track a remote branch (see the next section and Git Branching for more information), you can use the `git pull` command to automatically fetch and then merge that remote branch into your current branch. This may be an easier or more comfortable workflow for you; and by default, the `git clone` command automatically sets up your local `master` branch to track the remote master branch (or whatever the default branch is called) on the server you cloned from. Running git pull generally fetches data from the server you originally cloned from and automatically tries to merge it into the code you’re currently working on.

Pushing to Your Remotes
----------------------
When you have your project at a point that you want to share, you have to push it upstream. The command for this is simple: `git push <remote> <branch>`. If you want to push your master branch to your origin server (again, cloning generally sets up both of those names for you automatically), then you can run this to push any commits you’ve done back up to the server:

```$ git push origin master```
This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to fetch their work first and incorporate it into yours before you’ll be allowed to push. See Git Branching for more detailed information on how to push to remote servers.

Inspecting a Remote
--------------------
If you want to see more information about a particular remote, you can use the `git remote show <remote>` command. (**Frequently used**)

```
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master                         (up to date)
```

Renaming and Removing Remotes
-----------------------------
You can run `git remote rename` to change a remote’s shortname.
If you want to remove a remote for some reason — you’ve moved the server or are no longer using a particular mirror, or perhaps a contributor isn’t contributing anymore — you can either use `git remote remove` or `git remote rm`.

Listing Your Tags
-----------------
Listing the existing tags in Git is straightforward. Just type `git tag` (with optional `-l` or `--list`).

Creating Tags
-------------
Git supports two types of tags: **lightweight** and **annotated**.

Annotated Tags
--------------
Creating an annotated tag in Git is simple. The easiest way is to specify `-a` when you run the tag command:

`$ git tag -a v1.4 -m "my version 1.4"`

Lightweight Tags
---------------
Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file — no other information is kept. To create a lightweight tag, don’t supply any of the `-a`, `-s`, or `-m` options, just provide a tag name:

`$ git tag v1.4-lw`

Tagging Later
-------------
To tag a commit, you specify the commit checksum (or part of it) at the end of the command:

`$ git tag -a v1.2 9fceb02`

Then, you can `git tag` and `git show v1.2`

Sharing Tags
------------
By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run `git push origin <tagname>`.

If you have a lot of tags that you want to push up at once, you can also use the `--tags` option to the git push command. This will transfer all of your tags to the remote server that are not already there.

```$ git push origin --tags```

Deleting Tags
--------------
To delete a tag on your local repository, you can use `git tag -d <tagname>`. 

Note that this does not remove the tag from any remote servers. There are two common variations for deleting a tag from a remote server.

The first variation is `git push <remote> :refs/tags/<tagname>`:
```
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
```
The way to interpret the above is to read it as the null value before the colon is being pushed to the remote tag name, effectively deleting it.

The second (and more intuitive) way to delete a remote tag is with:

```$ git push origin --delete <tagname>```

Checking out Tags
If you want to view the versions of files a tag is pointing to, you can do a `git checkout` of that tag, although this puts your repository in “**detached HEAD**” state, which has some ill side effects.

In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:

```
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
If you do this and make a commit, your `version2` branch will be slightly different than your `v2.0.0` tag since it will move forward with your new changes, so do be careful.

Git Aliases
-----------
If you don’t want to type the entire text of each of the Git commands, you can easily set up an alias for each command using git config. For example,

`$ git config --global alias.co checkout`





