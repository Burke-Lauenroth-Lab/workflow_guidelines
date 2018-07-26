# Git Cheatsheet

* The 4 levels of the git organization
    * __Working directory__ (on local computer): work in text editor and/or an IDE (RStudio, Visual Studio, etc.)
    * __Staging area__: include/save changes to the next commit
    * __Local repository__: commit to project history
    * __Remote repository__ on github.com: share code with collaborators and backup local branches

* __Inspecting__ a repository
    * State of working directory and staging area: git status
    * History of commits: `git log --graph --full-history --oneline --decorate`
    * List of branches (* indicates the active branch):
    * Local branches: `git branch`
    * Remote branches: `git branch -r`
    * List of remote connections: `git remote -v`

* __Finding stuff__ in a repository
    * Find all commits which have affected a file: `git log -- *<part_of_file_name>*`
    * Find the SHA of the last commit that affected a file `git rev-list -n 1 HEAD -- <file_path>`
    * Find all commits which have deleted files and list the deleted files:
      `git log --diff-filter=D --summary`

* __Remove commits__ from current (private, i.e., not published on remote repository) state of a branch (i.e., re-writing history)
    * From working directory, staged snapshot, and commit history: `git reset --hard HEAD~1`
    * From staged snapshot and commit history: `git reset --mixed HEAD~1`
    * From commit history: `git reset --soft HEAD~1`
* __Remove commits__ from current published branch (by creating a new commit, i.e., it does not re-write history): `git revert HEAD~1`

* __Restore file__ from a previous commit: `git checkout <deleting_commit>~1 -- <file_path>`
* __Amending the most recent commit message__ (see [SO](http://stackoverflow.com/questions/179123/how-to-modify-existing-unpushed-commits) for alternative scenarios)
    * Completely rewrite message from scratch: `git commit --amend`
    * Amend by starting from old message: `git commit --amend -c HEAD`
* __Interruptions__ to coding: 'stashing' saves uncommitted changes and resets/cleans the working directory, e.g., to switch branches, to pull into a dirty tree, to interrupt the workflow in general. Stashes are handled in the same way as commits by git commands, but they are not linked to a specific branch. Stashes are named <stash@{X}> where X is the number on the stack. For more details see [here](https://git-scm.com/docs/git-stash) and [here](https://git-scm.com/book/tr/v2/Git-Tools-Stashing-and-Cleaning)
    * Push a new stash onto stack: `git stash` (this will only stash files that are already tracked); to stash also untracked (i.e., new files): `git stash --include-untracked`
    * List stored stashes on stack: `git stash list`
    * Apply a stored stash: `git stash apply` will apply <stash@{0}>; apply stash with number X: `git stash apply stash@{X}`. Git gives merge conflict messages if a stash does not apply cleanly. Apply a stash and stage files as before: `git stash apply --index`
    * Remove a stash from the stack: `git stash drop stash@{X}`
    * Apply and remove a stash: `git stash pop`
    * Show what applying a stash would add/remove to <branch>: `git diff <branch> stash@{X}`
