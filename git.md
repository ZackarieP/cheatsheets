# Git Cheatsheet

## Keeping a topic branch on a fork up to date
```shell
git checkout master
# Pull new changes from upstream/master
git pull upstream master

# Push changes from upstream/master to origin/master
git push origin master

# Merge changes from master into topic branch and push to origin/<topic branch>
git checkout <topic branch>
git merge master
git push origin <topic branch>
```

## Configuring remotes
```shell
# Add a new remote
git remote add <remote name> <url>

#Change the URL of an existing remote
git remote set-url <remote name> <url>
```

## Configuring branches
```shell
# Create new local branch
git checkout -b <branch name> # This new branch will be based on whatever branch you were previously on

# Publish local branch to any remote
git push -u <remote name> <branch name>

# Delete local branch
git branch -d <branch name>

# Delete remote branch
git push --delete <remote name> <branch name> 
```