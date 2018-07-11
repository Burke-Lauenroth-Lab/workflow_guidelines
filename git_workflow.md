## Workflow Guidelines <a name="workflow"/>

1.	Set __global user options__ to identify your commits
    * `git config --global user.name <name>`
    * `git config --global user.email <email>`
    * `git config --global core.editor <editor>` # e.g., vi, emacs, nano, TextWrangler (Microsoft Windows user refer to [First-Time-Git-Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup))
    * `git config --global merge.conflictstyle diff3` # conflict resolution with three sections: HEAD (code between `<<<<<<<` and `|||||||`), feature-branch  (code between `=======` and `>>>>>>>`), and (3rd) merged (=last) common ancestor (code between `|||||||` and `=======`)
    * Activate two-factor authentication for your account on [github.com](https://help.github.com/articles/providing-your-2fa-authentication-code/)
        - DRS prefers using a TOTP application over text messaging, e.g., [Duo Mobile](https://duo.com/solutions/features/two-factor-authentication-methods).
        - Command line tools will require a [personal access token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/) instead of your regular password.
        - Instead of repeatedly entering the token, you could enable 'git credential caching' with `git config --global credential.helper cache` (on Linux) or `git config --global credential.helper osxkeychain` (on macOS).

2. Create a __new branch__ each time you start to develop new functionality or work on improving code.
    1. Get a copy of a remote repository to your local computer:
        * `git clone https://github.com/Burke-Lauenroth-Lab/SOILWAT2.git`
        * `git clone https://github.com/Burke-Lauenroth-Lab/rSFSW2.git`
        * Get a copy of a specific branch:
            * `git clone -b bugfix_16 https://github.com/Burke-Lauenroth-Lab/SOILWAT2.git`
        * If the repository contains sub-modules:
            * `git clone --single-branch --recursive https://github.com/Burke-Lauenroth-Lab/rSOILWAT2.git rSOILWAT2`
    2. Make a new branch and check it out: `git checkout -b <branch>`
    3. Push/export local <branch> to remote/upstream: `git push origin <branch>`

3. Create a __milestone__ that describes the purpose of the branch

4. Work on code
    * Whenever a significant enhancement or issue arises, or when you think one will arise in the future, document it via an issue, and assign that issue to the milestone.
        * A good rule of thumb is that other group members should know what you are working on at any given point in time. After initial functionality has been developed, you should aim to document every significant change that will need to be done before the branch can be merged to master. [Close the issue](#closing-issues) when it has been resolved.
    * __Stage__ your changes in a snapshot: `git add <file>` or `git add <directory>` or `git add -p`
    * Navigation
        * Between branches: `git checkout <branch>`
        * Between commits: `git checkout <commit>` # HEAD points to <commit> in a 'detached HEAD' state (i.e., view but do not edit!)
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
    * __Resolve merge/rebase conflicts__ (see, e.g., the section 'How To Resolve Conflicts' of [git-merge](https://git-scm.com/docs/git-merge),  [here](https://githowto.com/resolving_conflicts), or [here](https://developer.atlassian.com/blog/2015/12/tips-tools-to-solve-git-conflicts/)): DRS uses a GUI merge tool (TextWrangler or kdiff3) by issuing `git mergetool -t kdiff3`. Several other tools are available, see [here](https://www.slant.co/topics/1324/~diff-tools-for-git), [here](https://www.quora.com/What-is-the-best-git-merge-tool-for-The-Mac), and [here](https://developer.atlassian.com/blog/2015/12/tips-tools-to-solve-git-conflicts/) for a comparison and discussion of pros and cons).

5. __Commit__ to your development branch regularly and use explanatory commit messages in order to create a transparent work history (e.g., to help with debugging; to find specific changes at a later time). Each commit is a separate logical unit of change and is therefore composed of related changes.
    * Check state of staging area: `git status`
    * Commit/save to project history:
        * Commit staged snapshot: `git commit -m "<message>"` # where <message> contains the commit description on the first line (< 50 characters), a blank line, and a detailed description (include keywords to close/fix/resolve issues, e.g., closes #45 will close issue #45 in the repository)
    * Invoke a text editor to compose message: `git commit`
    * _Strongly_ follow [these guidelines](https://github.com/erlang/otp/wiki/Writing-good-commit-messages) for writing a message
    * Commit all changes: `git commit -am "<message>"`

6. Share and backup your development commits on the remote repository. Our standard method for publishing local contributions to the github.com repository:
    0. Make sure you are on the development branch: `git checkout <branch>`
    1. Make sure the staging area is clean: `git status`
    2. Make local commit(s) to the development branch (see previous step)
    3. In case someone else is working on the same development branch, then import/merge new commits from remote/upstream to local branch: `git pull origin <branch>`
    4. In case the master branch has changed considerably or contains important updates, then rebase/merge with master
        - `git rebase master` or `git merge master`
        - remove/resolve conflicts, mark the resolved files with `git add` or `git rm`, and continue with `git rebase --continue` or `git merge --continue`
        - and conclude `git commit -am "<message>"`
    5. Push/export local project history to remote/upstream: `git push origin <branch>`


7. Repeat steps 3-5 until the development branch is ready for deployment. Open a [pull request](https://help.github.com/articles/creating-a-pull-request/) and ask for feedback from the team members. It usually does not hurt if someone else than the developer does a test on a feature, since another person may test differently.

8. If necessary, repeat steps 3-5 to complete review of the pull request, e.g. to deal with issues and fix bugs.

9. After a team member has reviewed the development branch, you should deploy the development branch and merge/rebase to the master. Our standard method with two options for deploying a development/feature branch to the master branch on github.com repository (option (i) with rebasing is ideal for small development branches or for simultaneous work on same code section; option (ii) with merging is preferred for large development branches; see following stackoverflow discussions [here](http://stackoverflow.com/questions/1241720/git-cherry-pick-vs-merge-workflow) and [here](http://stackoverflow.com/questions/457927/git-workflow-and-rebase-vs-merge-questions)):
    0. Make sure you are on the development branch: `git checkout <branch>`
    1. Make sure the staging area is clean: `git status`
        * Note: The rSOILWAT2 repository will always show 'src' as 'untracked content' because SOILWAT2 is loaded as a submodule. Don't stage and commit 'src'. If you want to edit the code of SOILWAT2, do that in the SOILWAT2 repository and then update the submodule in Rsoilwat as explained in the README of rSOILWAT2.
    2. If the repository is a R package, then adjust the lines 'Version' and 'Date' of the file DESCRIPTION to reflect the new version and potentially adjust package startup message in function '.onAttach' of the file R/zzz.R
    3. Option (i): Rebase development branch onto the tip of the master branch (given that there are no branches on <branch>): `git rebase master`
    4. Options (i) and (ii): Integrate with the main code base:
        1. `git checkout master`
        2. `git pull origin`
        3. `git merge <branch>`
    5. Resolve potential conflicts
    6. Commit and push the merge to remote/upstream with a detailed <message> particularly when using option (ii)
        1. `git commit -am "<message>"`
        2. `git push origin`
    7. Create an annotated version tag using semantic versioning with a format like v1.0.4
        * Tag the current commit
            * Use `git tag -a v1.0.4 -m "<message>"` and push the tag with `git push origin --tags`
            * Alternatively, use the webinterface to add a [new release](https://help.github.com/articles/creating-releases/) against master
        * Tag an old commit retroactively: you should do that so that the tag's date/time corresponds to the commit's date/time by temporarily setting the tag's clock:
            ```
            git checkout <branch>
            git reset --hard <commit SHA1>
            GIT_COMMITTER_DATE="$(git show --format=%aD  | head -1)" git tag -a v1.0.4 -m "<message>"
            git push --tags
            git pull
            ```
    8. Delete the development branch
        * Delete the local branch: `git branch -d <branch>`
        * Delete the remote branch: `git push origin --delete <branch>`
        * Remove 'obsolete tracking branches', i.e., branches on local machine that no longer exist on remote/github: `git fetch --all --prune`

* We use __[semantic versioning](http://semver.org/)__ using the format MAJOR.MINOR.PATCH. Every commit to the master branch updates the version number. A backwards-incompatible commit increases MAJOR and resets MINOR and PATCH to 0. A backwards-compatible commit adding new functionality increases MINOR and resets PATCH to 0. A backward-compatible commit fixing bugs (etc) increases PATCH.

## Git Basics <a name="basics"/>

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
