---
date: 2020-07-14 11:28:58-07:00
excerpt: Everyone has their own Git guide. This is mine.
excerpt_standalone: true
layout: post
title: Git branch-based contribution workflow management
---
Let's face it, Git is not easy to use.  Actually, `git clone $URL` and then the occasional `git pull` is simple, but the barrier to entry for making changes is quite high.  Even if a novice makes it to the point of filing a pull request, a response like "please squash your changes" probably makes no sense.

This post is roughly based on a guide I wrote for an internal company wiki about a decade ago.  It describes a workflow for making contributions to an existing Git-managed project, using purposeful branches.  I say "purposeful" a lot; the idea is a branch houses the development of a single change, which has a single purpose.  99% of the time, that change will be a single commit by the time it's reviewed and merged upstream.

Managing purposeful changes in their own branches lets you manage multiple contributions to a single project.  There was a project last year where I had over 20 changes ready to be reviewed and merged, each in their own branch.  Most of them were simple and could be reviewed in a minute, but since they were split up into dedicated changes, you didn't have to coordinate with the reviewer about which requests to review in what order (most of the time).

As an example, let's use the [Git repository](https://github.com/rfinnie/2ping) for [2ping](https://www.finnie.org/software/2ping/), a somewhat popular utility I wrote.  First, we want to clone the origin repository.

```
$ git clone https://github.com/rfinnie/2ping
Cloning into '2ping'...
$ cd 2ping/
2ping{main}$
```

2ping's primary branch is "main", as are a growing number of repositories as of 2020, but keep in mind the default primary branch for Git repositories is "master", so most repositories you will come across use that.

Now, if you want to make a change, the first thing you want to do is create a new, purposeful branch.  In this example, we want to add a new file, so let's call the branch "addfile".

```
2ping{main}$ git checkout -b addfile
Switched to a new branch 'addfile'
2ping{addfile}$
```

The examples here show the current branch between curly brackets ([you can do the same with your own PS1 prompt](https://git-scm.com/book/cs/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash)), but you can always see which branch you are currently on.

```
2ping{addfile}$ git branch
* addfile
  main
```

Now, let's get editing! Commit early and commit often, and don't worry about getting everything right with each commit.  We will be squashing everything down to a single, purposeful commit at the end, so right now the development commits are more of a stream-of-consciousness log for your own benefit.

```
2ping{addfile}$ echo foo > bar
2ping{addfile}$ git add bar
2ping{addfile}$ git commit -m 'Add a new file, "bar"'
2ping{addfile}$ echo dive > bar
2ping{addfile}$ git commit -a -m 'Oops, this is what the new file should look like'
```

Other people may be working on the repository at the same time as you, and new commits may be added to the primary branch between the time you start the new branch and the time you're ready for review.  Occasionally, you want to pull in any new commits, and integrate them into your working branch.

```
2ping{addfile}$ git fetch origin main
2ping{addfile}$ git rebase origin/main
```

The first command fetches any new commits for the "main" branch from the "origin" remote (when you did a `git clone` originally, the URL you specified became the "origin" remote by default).  The second command will take the "main" branch as a base, and apply any commits you made to the "addfile" branch afterward.

There are several commands available which give you an idea of the current status of your branch in relation to the primary branch.

```
2ping{addfile}$ git diff origin/main
2ping{addfile}$ git log origin/main..HEAD
```

Commit often, but remember to keep the actual branch's scope limited to a single purposeful change.  Say you're in the middle of this addfile change and you notice a typo in README.md.  It's easy to commit your work in progress, then switch to a yet another new branch off the primary branch and update README.md.

```
2ping{addfile}$ git commit -a -m 'Work in progress commit'
2ping{addfile}$ git checkout main
2ping{main}$ git pull
2ping{main}$ git checkout -b readme-typo
2ping{readme-typo}$ vi README.md
2ping{readme-typo}$ git commit -a -m 'Fix README.md typo'
2ping{readme-typo}$ git checkout addfile
2ping{addfile}$
```

(`git stash` is also available to do roughly the same thing, but as you're already working with a dedicated branch which will be squashed, it's actually easier to just commit your WIP.)

Once you're ready, you'll have a few commits for a single change.  You'll want to squash those down into a single commit.

```
2ping{addfile}$ git rebase -i main
```

This will open an editor with all of your commits since the branch point, with the oldest commit at the top.

```
pick 9e669cc36 Add a new file, "bar"
pick ab0c50f93 Oops, this is what the new file should look like
```

Edit the commands so you squash all other commits into the top commit.
```
pick 9e669cc36 Add a new file, "bar"
squash ab0c50f93 Oops, this is what the new file should look like
```

Once you save and exit, another editor will open for the final commit message.  It will contain the text of all your commits, so you can pare the text down into what you want the final commit message to be.

Now you've got a single, purposeful commit with the desired change.
```
2ping{addfile}$ git show
commit 67017eed63b41cc57a6c81fe9e0be9ded733be30 (HEAD -> addfile)
Author: Ryan Finnie <ryan@finnie.org>
Date:   Mon Jul 13 15:34:42 2020 -0700

    Add a new file, "bar"

    This file is vital to the operation of 2ping.

diff --git a/bar b/bar
new file mode 100644
index 0000000..1bc5169
--- /dev/null
+++ b/bar
@@ -0,0 +1 @@
+dive
```

Now you'll want to push it to a personal repository.  We'll use GitHub as an example here, but unfortunately GitHub does not allow pushing to new namespaces, so you'll need to clone the origin repository through the web site first.  Once that's done, you can add your clone of the repository as a new "remote".
```
2ping{addfile}$ git remote add personal https://github.com/youruser/2ping
```

If you remember above, new data was being checked for via the "origin" remote.  This new "personal" remote is simply adding a second, non-default remote to your local repository.  You only need to add this remote to your local repository once.

Now you can push your new branch to your personal remote.

```
2ping{addfile}$ git push personal addfile
```

At this point, GitHub will notice that this is a new branch in a clone of a third-party repository, and will give you a URL which lets you create a pull request against the origin.

Say you create a pull request and the upstream requests alterations.  Go ahead and make them, then rebase squash against the primary branch again.

```
2ping{addfile}$ echo gold > bar
2ping{addfile}$ git commit -a -m 'Upstream does not like dive bars, and instead wants gold'
2ping{addfile}$ git rebase -i origin/main
```

Now push again, but since you are pushing a change which cannot be fast-forwarded on your remote "addfile" branch (remember, rebasing effectively alters history), you'll need to force the push.

```
2ping{addfile}$ git push personal addfile --force
```

While this is the preferred workflow for working branches for the purpose of submitting changes, you should **never** rebase a primary branch, as it alters history and makes it very hard for others to work with your repository.

Now, if your pull request is accepted and merged, you can go back to the primary branch and pull in the changes from the default "origin" remote.

```
2ping{addfile}$ git checkout main
2ping{main}$ git pull
```

Congratulations, you've navigated a change workflow!  Now, you can apply this workflow to local development as well.  Even if the repository is yours, you can use branches to manage your development.  For a complex personal project, I'll often have dozens of half-implemented branches in various states of work.  When a change is ready, all you need to do is rebase against the primary branch, then switch to it and merge.

```
2ping{addfile}$ git checkout main
2ping{main}$ git merge addfile
2ping{main}$ git push
```