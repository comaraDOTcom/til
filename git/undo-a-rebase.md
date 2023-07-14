Sometimes (read: often if you're me), a git rebase doesn't go to plan. During the rebase, every commit after your resolved conflicts end up getting committed again. You push them and the PR looks a mess.

To undo the operation we can follow, call `git reflog`

```bash
cda11d5 HEAD@{0}: rebase: commit 1 
8272fde HEAD@{1}: rebase: commit 2 
dc12a9c HEAD@{2}: checkout: your last commit of work
```

To go back to your last commit here: 
```bash 
git reset --hard HEAD@{2}
```

Now the history on your local branch will vary from the remote's, so a `force` push will be required.
```bash
git push -f
```
