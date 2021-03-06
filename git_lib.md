# Git Lib
=========<br>
Basic git lib.

1. My code is ready to be pushed
```
cd existing-project
git init
git add --all
git commit -m "Initial Commit"
git remote add origin ssh://git@10.10.10.10/one_directory/one_repo.git
git push -u origin master
```
2. My code is already tracked by Git
```
cd existing-project
git remote set-url origin ssh://git@10.10.10.10/one_directory/one_repo.git
git push -u origin --all
git push origin --tags
```
3. Remove untracked files from the working directory
```
   git clean -f
   git reset --hard
```
4. Undoes a committed snapshot
```
   git revert
```
5. Error: cannot lock ref 'refs/remotes/origin...' -> clean up local repo
```
  git gc --prune=now
  git remote prune origin
```