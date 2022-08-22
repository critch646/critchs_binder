# Project workflow using Git and GitHub
To coordinate efforts as a team we will be using Git for our version control 
system and GitHub as our remote repository. Branches will be an integral 
component of our efforts.

## Git Hygiene
Here are a few good practices to utilize when using Git.

### Current Branch
Always know the current branch checked out before doing any work, whether it is
to edit files or merge another branch into the current. Know your branch! Make
`git status` your friend!

Personally, I will `checkout` my `developer branch` after pushing my changes, 
especially if I am finished coding for now. There will be times when I forget 
to check my current branch.

### Commits
It is best to make small, concise, and logical commits. When gradual changes are
made it is easier to find introduced bugs or rollback changes.

Detail what changes you made and *why*. Looking back at a commit (you or another
developer) it will be easier to understand what was trying to be accomplished
versus what was implemented.

### .gitignore
There will be numerous files that are not needed by other developers or might
be too big for the repo (video files). For the first, files such as IDE 
workspace files (`.vs`, `.idea`) is only needed for your specific environment. 

As for the latter, what project files that are not going to be held in the 
repository is agreed upon by the group. Such files will have to be distributed
by other means.

### Stashing
using `git stash` is a great way to keep your work without making a `commit` you
are not ready to make. Some use cases:
* You need to `checkout` another branch or a specific `commit`
* You want to try something experimental and potentially *dangerous*.
