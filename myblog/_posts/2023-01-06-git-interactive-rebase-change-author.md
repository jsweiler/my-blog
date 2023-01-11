---
layout: post
title:  "How To Change Git Commit Author"
date:   2023-01-06 06:04:29 -0400
categories: git
---

## How to change a git commit author

Have you ever commited code and later realized that your email or name was not correct? 
I recently discovered you can do this (and more things) using the interactive rebase feature of Git. 

> Note: there are many other ways to do this as well, this is what I found worked for me. Your experience may be different.

Also I am going to show you how to do this with the VS Code editor. By default I believe the git editor is set to vim. Some people really love vim but I don't happen to be one of those. If you do like vim just disregard the next step.

To determine which editor git has configured run this command:
```
git config global --list
```
 Look for the core.editor item in the results. Also instead of global you can use --local, --system or --worktree. 

To set your editor to VS code run this command
```
git config --global core.editor "code --wait"
```
For this illustration I'm going to show a situation where I made 3 commits to a previous repo with the wrong git config username and also pushed remotely and show how to change that. 

What you need to do is find the last good commit id from history and copy that. You can use `git log` to show history. In my case the last 3 commits are the bad ones. (The ones with Full Name as the author).

![Git Log ](/assets/images/2023-01-06 15_04_46-GitDemo.png)

Then you will want to run this command.
```
git rebase -i -p <previous-commit-id>
```

If you set your git config editor to VS Code you should see a window pop up now.

You should see VS code pop up a window like this:
![](/assets/images/2023-01-06 15_56_21-git-rebase-todo - _posts - Visual Studio Code)

What you will do is change pick to edit and close the window.
Then git will go through each of the commits that it showed in your terminal giving you a chance to change it. We will want to run the following command to change the incorrect username:
```
git commit --amend --author="John Doe <correct-email>" --no-edit
```
Then run 
```
git rebase --continue
```
And repeat the amend step for each commit.

And remember I said for my scenario I had pushed the commits remotely so I need to run this command to force push my new ammended commits
```
git push -f origin <branch-name>
```

And you are done! While it may seem tricky at first I find that this is a fairly easy way to change the author for commits. Just pay attention while you are running the commands though! It is easy to make mistakes when running more advanced git commands.