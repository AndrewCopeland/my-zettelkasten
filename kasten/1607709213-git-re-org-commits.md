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

For each line (commits) you can do different possible actions:
- `fixup`: Command tells git to ammend the commit without changing the message
- `squash`: Command tells git to amend the commit and ask in an editor which is the message of the new commit
- 



## Links
