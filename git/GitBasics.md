# Git Basics
Regards a project as entirely contained in the same directory (repo).
* `$ git init`   # creates a .git directory with all the info about the repo.
* Want to be able to specify what files we want to store updated info about the repo.
* I create or modify a file I want to keep
	- `$ git add [filename]`
* When I reach a point in the overall project
	- `$ git commit -m "blah blah"` Quick description of current state
	- `$ git status` tells you of untracked files (changes not added) and of uncommitted adds, and unpushed commits.
* If we are all working from a common repo:
	- `$ git push` pushes commits back to repo cloned from.
	- `$ git mv oldname newname` rename/move files in repo
	- `$ git rm filename` remove file.
	- `$ git stash` you can stash the changes since the last commit
		+ Puts the changes into a stock of stashes
		+ Puts them back the way they were
	- `$ git stash pop` brings back the changes
	- `$ git stash drop` discards the most recent stash
* `$ git pull` looks at the repo you cloned and pulls any new changes
* remote can be used to specify another place to pull from
	- `$ git remote add [our name for remote location]:[remote location]`

Suppose I've edited a file locally, X, and added and committed, and X has changed on the remote, git flags that file as a conflict and expects you to fix it and add the fix.

* You remove the code you don't want, and then you do a ` $ git add`
* `$ git fetch [instructor]`
	* `$ git diff HEAD [instructor] [master]` tells you what is different between you and the remote
	* merge manually
+ `$ git merge [instructor] [master]`

## Branches
We may need to support multiple versions of a project. I want to be able to switch between the different versions.
* right now give me the files for version 2
* We want to be able to create versions and swap between them
	- `$ git branch [some new name]` creates a new branch from current branch
	- `$ git checkout [some new name]` switches files to that new branch
		+ **Do a `$ git status` before branch checkout and do any necessary adds/commits**
	- `$ git branch` lists branches
	- `$ git merge [other branch]` merges other branch into current branch

## Viewing Commits
* gui tool `gitk` lets you explore commits and state of files.
* text version
	- `$ git log --graph --pretty --branches`

## Pushing ALL Branches
* `$ git push origin --all` pushes all of your branches

## Deleting Branches
* Often as our project continues along and we create branches to squash a bug or implement a feature.
* After we are done it is a good idea to delete your unused branches to make your repo more managable.
* `$ git branch -d <branch name>` will delete your local branch
* `$ git push <remote> --delete <branch>` will delete the branch at the remote site.
* Alternatively, you can delete a remote branch using `git push <remote> :<branch>`
* You will want to synchronize your branch list, so it is a good idea to prune using:
	- `$ git fetch -p` 
