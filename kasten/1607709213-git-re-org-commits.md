# 1607709213 git-re-org-commits
It is possible to rebase interactively using the following command:
```bash
git rebase --interactive HEAD~7
```

This will open an editor with the `7` shown commits (in the opposite order)

```
pick 18d8ace Added new awesome feature
pick ccc50cc fix npe stuff
pick 5a03bf5 fix another bug
pick 24fe90a Merge from somewhere else
pick 84aaad2 wip
pick eed23a1 Complete awesome feature with better error message
pick a2d6ee9 Review fixes
```

All you need to do is change the work `pick` to the desired command:
```
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
```


If I have a lot of small commits that can easily be squashed into one I use the `fixup` command.
```
pick 18d8ace Update README
fixup ccc50cc Update README 1
fixup 5a03bf5 Update README 2
fixup 24fe90a Update README 3
```

This will squash the 3 bottom commits into the first commit `Update README`.


## Links
