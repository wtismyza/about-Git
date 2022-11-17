Distributed Workflows
---------------------
1. **Centralized Workflow**
2. **Integration-Manager Workflow**: **The contributor sends the maintainer an email asking them to pull changes, how. fork?**
3. **Dictator and Lieutenants Workflow**

Contributing to a Project
-------------------------
**Commit Guidelines**: Before you commit, run `git diff --check`, which identifies possible whitespace errors and lists them for you.

**How to reverse some actions, how to maintain a large project, how to merge and rebase are some tricky issues.**

**more advanced actions to learn...**

create pull request

Other notes
----------------

If you pass `--abbrev-commit` to the git log command, the output will use shorter values but keep them unique; it defaults to using seven characters but makes them longer if necessary to keep the SHA-1 unambiguous.

RefLog Shortnames
One of the things Git does in the background while you’re working away is keep a “reflog” — a log of where your HEAD and branch references have been for the last few months.

You can see your reflog by using `git reflog`:
```
$ git reflog
734713b HEAD@{0}: commit: Fix refs handling, add gc auto, update tests
d921970 HEAD@{1}: merge phedders/rdocs: Merge made by the 'recursive' strategy.
1c002dd HEAD@{2}: commit: Add some blame and merge stuff
1c36188 HEAD@{3}: rebase -i (squash): updating HEAD
95df984 HEAD@{4}: commit: # This is a combination of two commits.
1c36188 HEAD@{5}: rebase -i (squash): updating HEAD
7e05da5 HEAD@{6}: rebase -i (pick): updating HEAD
Every time your branch tip is updated for any reason, Git stores that information for you in this temporary history. You can use your reflog data to refer to older commits as well. For example, if you want to see the fifth prior value of the HEAD of your repository, you can use the @{5} reference that you see in the reflog output:
```
`$ git show HEAD@{5}`
You can also use this syntax to see where a branch was some specific amount of time ago. For instance, to see where your master branch was yesterday, you can type:

`$ git show master@{yesterday}`

To see reflog information formatted like the git log output, you can run `git log -g`:

```$ git log -g master```

Ancestry References
-------------------
The other main way to specify a commit is via its ancestry. If you place a ^ (caret) at the end of a reference, Git resolves it to mean the parent of that commit. Suppose you look at the history of your project:

Then, you can see the previous commit by specifying `HEAD^`, which means “the parent of HEAD”:

`$ git show HEAD^`

You can also specify a number after the `^` to identify which parent you want; for example, `d921970^2` means “the second parent of d921970.” This syntax is useful only for merge commits, which have more than one parent — the first parent of a merge commit is from the branch you were on when you merged (frequently `master`), while the second parent of a merge commit is from the branch that was merged:

The other main ancestry specification is the `~` (tilde). This also refers to the first parent, so `HEAD~` and `HEAD^` are equivalent. The difference becomes apparent when you specify a number. `HEAD~2` means “the first parent of the first parent,” or “the grandparent” — it traverses the first parents the number of times you specify.

Commit Ranges
-------------
Now that you can specify individual commits, let’s see how to specify ranges of commits.

The most common range specification is the double-dot syntax. This basically asks Git to resolve a range of commits that are reachable from one commit but aren’t reachable from another.

Say you want to see what is in your experiment branch that hasn’t yet been merged into your master branch. You can ask Git to show you a log of just those commits with master..experiment — that means “all commits reachable from experiment that aren’t reachable from master.” For the sake of brevity and clarity in these examples, the letters of the commit objects from the diagram are used in place of the actual log output in the order that they would display:

```$ git log master..experiment```

```$ git log origin/master..HEAD```
This command shows you any commits in your current branch that aren’t in the master branch on your origin remote. If you run a git push and your current branch is tracking origin/master, the commits listed by `git log origin/master..HEAD` are the commits that will be transferred to the server. You can also leave off one side of the syntax to have Git assume HEAD. For example, you can get the same results as in the previous example by typing git log origin/master.. — Git substitutes `HEAD` if one side is missing.

Multiple Points
The double-dot syntax is useful as a shorthand, but perhaps you want to specify more than two branches to indicate your revision, such as seeing what commits are in any of several branches that aren’t in the branch you’re currently on. Git allows you to do this by using either the `^` character or `--not` before any reference from which you don’t want to see reachable commits. Thus, the following three commands are equivalent:
```
$ git log refA..refB
$ git log ^refA refB
$ git log refB --not refA
```
This is nice because with this syntax you can specify more than two references in your query, which you cannot do with the double-dot syntax. For instance, if you want to see all commits that are reachable from `refA` or `refB` but not from `refC`, you can use either of:
```
$ git log refA refB ^refC
$ git log refA refB --not refC
```
This makes for a very powerful revision query system that should help you figure out what is in your branches.

Triple Dot
--------
The last major range-selection syntax is the triple-dot syntax, which specifies all the commits that are reachable by **either** of two references but not by both of them. Look back at the example commit history in Example history for range selection. If you want to see what is in master or experiment but not any common references, you can run:

```$ git log master...experiment```
A common switch to use with the log command in this case is `--left-right`, which shows you which side of the range each commit is in.


Interactive Staging
-------------
In this section, you’ll look at a few interactive Git commands that can help you craft your commits to include only certain combinations and parts of files. These tools are helpful if you modify a number of files extensively, then decide that you want those changes to be partitioned into several focused commits rather than one big messy commit. This way, you can make sure your commits are logically separate changesets and can be reviewed easily by the developers working with you.

If you run `git add` with the `-i` or `--interactive` option, Git enters an interactive shell mode, displaying something like this:
```
$ git add -i
           staged     unstaged path
  1:    unchanged        +0/-1 TODO
  2:    unchanged        +1/-1 index.html
  3:    unchanged        +5/-1 lib/simplegit.rb

*** Commands ***
  1: [s]tatus     2: [u]pdate      3: [r]evert     4: [a]dd untracked
  5: [p]atch      6: [d]iff        7: [q]uit       8: [h]elp
What now>
```










