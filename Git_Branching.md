Branches in a Nutshell
----------------------
As you may remember from What is Git?, Git doesn’t store data as a series of changesets or differences, but instead as a series of snapshots.

When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the content you staged. This object also contains the author’s name and email address, the message that you typed, and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one parent for a normal commit, and multiple parents for a commit that results from a merge of two or more branches.

To visualize this, let’s assume that you have a directory containing three files, and you stage them all and commit. Staging the files computes a checksum for each one (the SHA-1 hash we mentioned in What is Git?), stores that version of the file in the Git repository (Git refers to them as **blobs**), and adds that checksum to the staging area:

When you create the commit by running git commit, Git checksums each subdirectory (in this case, just the root project directory) and stores them as a tree object in the Git repository. Git then creates a commit object that has the metadata and a pointer to the root project tree so it can re-create that snapshot when needed.

If you make some changes and commit again, the next commit stores a pointer to the commit that came immediately before it.

A branch in Git is simply a lightweight movable pointer to one of these commits. The default branch name in Git is `master`. As you start making commits, you’re given a `master` branch that points to the last commit you made. Every time you commit, the `master` branch pointer moves forward automatically.

Creating a New Branch
-------------------

Let’s say you want to create a new branch called testing. You do this with the `git branch` command:

```$ git branch testing```
This creates a new pointer to the same commit you’re currently on.

How does Git know what branch you’re currently on? It keeps a special pointer called `HEAD`. Note that this is a lot different than the concept of HEAD in other VCSs you may be used to, such as Subversion or CVS. In Git, this is a pointer to the local branch you’re currently on. In this case, you’re still on master. The git branch command only **created** a new branch — it didn’t switch to that branch.

You can easily see this by running a simple `git log` command that shows you where the branch pointers are pointing. This option is called `--decorate`.

Switching Branches
------------------
To switch to an existing branch, you run the `git checkout` command. 

You can also see this easily with the `git log` command. If you run `git log --oneline --decorate --graph --all` it will print out the history of your commits, showing where your branch pointers are and how your history has diverged.

**git log doesn’t show all the branches all the time**
If you were to run git log right now, you might wonder where the "testing" branch you just created went, as it would not appear in the output.

The branch hasn’t disappeared; Git just doesn’t know that you’re interested in that branch and it is trying to show you what it thinks you’re interested in. In other words, by default, git log will only show commit history below the branch you’ve checked out.

To show commit history for the desired branch you have to explicitly specify it: git log testing. To show all of the branches, add --all to your git log command.

Because a branch in Git is actually a simple file that contains the 40 character SHA-1 checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline).

**Creating a new branch and switching to it at the same time**
It’s typical to create a new branch and want to switch to that new branch at the same time — this can be done in one operation with `git checkout -b <newbranchname>`.

From Git version 2.23 onwards you can use `git switch` instead of `git checkout` to:

1. **Switch to an existing branch**: `git switch testing-branch`.
2. **Create a new branch and switch to it**: `git switch -c new-branch`. The `-c` flag stands for create, you can also use the full flag: `--create`.
3. **Return to your previously checked out branch**: `git switch -`.


Basic Branching and Merging
---------------------------
**What you might need**:
1. `$ git checkout -b iss53`
This is shorthand for:
```
$ git branch iss53
$ git checkout iss53
```
2. `$ git merge`
3. `$ git branch -d hotfix`

Branch Management
-----------------
The `git branch` command does more than just create and delete branches. If you run it with no arguments, you get a simple listing of your current branches.
To see the last commit on each branch, you can run `git branch -v`
To see which branches are already merged into the branch you’re on, you can run `git branch --merged`
To see all the branches that contain work you haven’t yet merged in, you can run `git branch --no-merged`

Changing a branch name
-----------------------
Suppose you have a branch that is called `bad-branch-name` and you want to change it to `corrected-branch-name`, while keeping all history. You also want to change the branch name on the remote (GitHub, GitLab, other server). How do you do this?

Rename the branch locally with the `git branch --move` command:

```$ git branch --move bad-branch-name corrected-branch-name```
This replaces your `bad-branch-name` with `corrected-branch-name`, but this change is only local for now. To let others see the corrected branch on the remote, push it:

```$ git push --set-upstream origin corrected-branch-name```
Now we’ll take a brief look at where we are now:
```
$ git branch --all
* corrected-branch-name
  main
  remotes/origin/bad-branch-name
  remotes/origin/corrected-branch-name
  remotes/origin/main
```
Notice that you’re on the branch `corrected-branch-name` and it’s available on the remote. However, the branch with the bad name is also still present there but you can delete it by executing the following command:

```$ git push origin --delete bad-branch-name```
Now the bad branch name is fully replaced with the corrected branch name.

Changing the master branch name
--------------------------------

Rename your local master branch into main with the following command:

```$ git branch --move master main```
There’s no local master branch anymore, because it’s renamed to the main branch.

To let others see the new main branch, you need to push it to the remote. This makes the renamed branch available on the remote.

```$ git push --set-upstream origin main```
Now we end up with the following state:
```
$ git branch --all
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/main
  remotes/origin/master
```
Your local master branch is gone, as it’s replaced with the main branch. The main branch is present on the remote. However, the old master branch is still present on the remote. Other collaborators will continue to use the master branch as the base of their work, until you make some further changes.







