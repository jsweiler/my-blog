---
layout: post
title:  "Other uses of git interactive rebase"
date:   2023-01-11 06:04:29 -0400
categories: git
---

## More Git Interactive Rebase

This will be an extension of my previous post about using git interactive rebase to change the author of a git commit. If you followed through that you probably noticed that you can do a lot more than just change a commit author.

If you do a git interactive rebase you will see the following commands listed as a possibilty for you to use:
```
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <commit> = run command (the rest of the line) using shell
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
```

Those are fairly self explanatory but let's quickly walk through how to change a commit message.

Run `git log` to find the commit ID of the commit just before the one you want to change the message for.

Then run this:
```
git rebase -i -p <previousCommitID>
```

You should see something like this in your editor.
![](/assets/images/2023-01-11 15_46_40-git-rebase-todo.png)

Find the ones you want to change the commit message and change pick to reword. Save your editor and close it. Then for each reword it will re-open your editor and allow you to change the message. Save and close it when done.

For my example I altered a commit message for a commit that was pushed remotely so I will force push.
```
git push -f origin <branch-name>
```