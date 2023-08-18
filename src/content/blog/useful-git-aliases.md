---
title: "Useful git aliases"
description: "A collection of git aliases both simple and more complex that can boost your productivity"
pubDate: "Aug 18 2023"
---

If you use `git` on a daily basis, creating aliases for your most often used commands can save you a ton of time. It also means you don't have to remember all the different flags and options for each command, and paired with a decent terminal such as [Warp](https://warp.dev/) you'll even get auto-completion for your aliases! Nice.

A lot of this was inspired by a post by Harry Roberts of CSS Wizardry fame: [Little things I like to do with Git](https://csswizardry.com/2017/05/little-things-i-like-to-do-with-git/).

To adopt any of the below aliases, you will need to put them in your `~/.gitconfig` file. You can either do this manually, or you can use the `git config` command to add them one by one:

```bash
git config --global alias.<alias-name> <command>
```

#### Simple shortcuts

```bash
s = status # git s
co = checkout # git co <branch-name>
cb = checkout -b # git cb <branch-name>
acm = !git add . && git commit -m # git acm "commit message"
```

#### Nicely formatted diffs and logs

```bash
# git d, standard diff but colored and ignoring whitespace
d = !git diff --color --ignore-all-space

# git wd, highlights changed words, not changed lines
wd = !git diff --color --word-diff # git wd

# git ll, shows full log of current branch
ll = !git log --oneline --no-merges --color

# git cl, shows only your new commits compared to `main`
cl = !git log --oneline --no-merges --color main.. | tail -r | cat -n

# git dno, shows the names of all changed files
dno = !git diff --name-only --color main | cat

# git leaderboard, lists contributors by number of commits
leaderboard = shortlog -sn --all --no-merges
```

#### Publish your current local branch

```bash
# git publish
publish = !git push -u origin $(git rev-parse --abbrev-ref HEAD)
```

#### Find branches

```bash
# git find <search-term>, lists branch names that contain <search-term>
find = !git branch -a | grep $1

# git br, like `git branch` but with more detailed output
br = branch --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset) - %(contents:subject) %(color:green)(%(committerdate:relative)) [%(authorname)]' --sort=-committerdate
```

#### Edit your commit history

Most of these assume that you are comfortable with `git rebase` and force-pushing your changes. **Only use these on a local feature branch, not a shared branch such as `main`!**

```bash
# git wip, a quick way to commit all your changes
wip = !git add . && git commit -nm "WIP"

# git undo, undoes your last commit but keeps the changes
undo = !git reset HEAD~1

# git amend, adds staged changes to the previous commit
amend = commit --amend --no-edit

# git fixup <commit-hash>, creates a fixup commit for the given hash
fixup = commit --fixup

# git squash, merges your fixup commits with their target hashes
squash = rebase --autosquash --interactive main
```

#### Delete branches and reset changes

```bash
# git delete-local <branch-name>
delete-local = branch -D

# git delete-origin <branch-name>
delete-origin = push origin --delete

# git nuke, resets the current local branch to it's remote version
nuke = !git reset --hard @{upstream} && git clean -df && git pull
```
