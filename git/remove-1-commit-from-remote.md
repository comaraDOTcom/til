I pushed a commit to the wrong branch.

First step is to remove the commit locally on the branch. Then force push the local branch to the remote branch
```bash
git reset --soft HEAD~1
git push origin HEAD --force
```

