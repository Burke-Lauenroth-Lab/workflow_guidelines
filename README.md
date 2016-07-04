# Github workflow for SOILWAT, Rsoilwat, and SoilWat_R_Wrapper
------
Version: June 8, 2016
Authors: Alexander Reeder, Daniel Schlaepfer


We use the ['Github flow'](https://guides.github.com/introduction/flow/) (a 'feature branch workflow') as the basis for our projects. Suggestions and improvements are welcome. The workflow description is based on terminal/console 'git' commands; the GUI program ['GitHub Desktop'](https://desktop.github.com/) for Mac OSX and Microsoft Windows allows for visually friendly handling of pull requests, merges, commits, branches, and diffs, but lacks some advanced features (e.g., management of sub-modules).


## Basics
* The 4 levels of the git organization
    * __Working directory__ (on local computer): work in text editor and/or RStudio
    * __Staging area__: include/save changes to the next commit
    * __Local repository__: commit to project history
    * __Remote repository__ on github.com: share code with collaborators and backup local branches

* There are __two types of branches__: the master branch and development/feature branches. Any commit on the master branch is deployable and has a unique version number. Deployable for us means that commits are tested. Such commits are usually the result of merging/rebasing with a development branch. The previous de facto master branches 'Rsoilwat_v31' and 'wrapper_sw31' have been merged with the true master branches.

* __Testing__ involves at least that each line of code was executed and did not throw an error or stopped execution unexpectedly. However, writing re-usable unit test cases (for R code based on the package [testthat](https://cran.r-project.org/web/packages/testthat/index.html)) is the preferred way to test our code.

* __Documentation__: Comment the code well and write object documentation with [roxygen](http://r-pkgs.had.co.nz/man.html), if using R, or with [doxygen](http://www.doxygen.org), if using another programming language.

* __Style guide__: In development. For R coding style, DRS suggests to follow [Hadley Wickham's style guide for R](http://adv-r.had.co.nz/Style.html) with the deviation to use tabs (equivalent to 4 spaces) for indentation (instead of 2 spaces) [to increase readability]. There are many other R style guides including: [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml), [Bioconductor's Coding Style](https://www.bioconductor.org/developers/how-to/coding-style/), [rDatSci/rOpenSci's R Style Guide](https://github.com/rdatsci/PackagesInfo/wiki/R-Style-Guide), [R Coding Conventions by H. Bengtsson](https://docs.google.com/document/d/1esDVxyWvH8AsX-VJa-8oqWaHLs4stGlIbk8kLc5VlII/edit), [4D R code style guide](https://4dpiecharts.com/r-code-style-guide/)

* We use __[semantic versioning](http://semver.org/)__ using the format MAJOR.MINOR.PATCH. Every commit to the master branch updates the version number. A backwards-incompatible commit increases MAJOR and resets MINOR and PATCH to 0. A backwards-compatible commit adding new functionality increases MINOR and resets PATCH to 0. A backward-compatible commit fixing bugs (etc) increases PATCH.

* __Inspecting__ a repository
    * State of working directory and staging area: git status
    * History of commits: `git log --graph --full-history --oneline --decorate`
    * List of branches (* indicates the active branch): 
    * Local branches: `git branch`
    * Remote branches: `git branch -r`
    * List of remote connections: `git remote -v`

* __Error reporting__: If you come across a bug in our code, please do something about it.
    * You can report it as a new issue, e.g.,  [here](https://github.com/Burke-Lauenroth-Lab/Rsoilwat/issues) for Rsoilwat, and communicate with the person who committed the buggy code.
    * If you can fix the problem yourself, then can create a new branch, improve the code, test the code and check that the changes did not break anything, and create a pull request (the commit message can automatically close the issue number(s); see workflow below).


## Workflow
1.	Set __global user options__ to identify your commits
    * `git config --global user.name <name>`
    * `git config --global user.email <email>`
    * `git config --global core.editor <editor>` # e.g., vi, emacs, nano, TextWrangler (Microsoft Windows user refer to [First-Time-Git-Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup))

2. Create a __new (development) branch__ each time you start to develop new functionality or work on improving code. Use descriptive branch names (e.g., new-snowmelt)
    1. Get a copy of a remote repository to your local computer:
        * `git clone https://github.com/Burke-Lauenroth-Lab/SOILWAT.git`
        * `git clone https://github.com/Burke-Lauenroth-Lab/SoilWat_R_Wrapper.git`
        * Get a copy of a specific branch:
            * `git clone -b segfault1 https://github.com/Burke-Lauenroth-Lab/SOILWAT.git`
        * If the repository contains sub-modules:
            * `git clone --single-branch --recursive https://github.com/Burke-Lauenroth-Lab/Rsoilwat.git Rsoilwat_v31`
    2. Make a new branch: `git branch <branch>`
    3. Change into a branch: `git checkout <branch>`
    4. Push/export local <branch> to remote/upstream: `git push origin <branch>`

3. Work on code
    * Stage your changes in a snapshot: `git add <file>` or `git add <directory>` or `git add -p`
    * Navigation
        * Between branches: `git checkout <branch>`
        * Between commits: `git checkout <commit>` # HEAD points to <commit> in a 'detached HEAD' state (i.e., view but do not edit!)
    * Remove commits from current (private, i.e., not published on remote repository) state of a branch (i.e., re-writing history)
        * From working directory, staged snapshot, and commit history: `git reset --hard HEAD`
        * From staged snapshot and commit history: `git reset --mixed HEAD`
        * From commit history: `git reset --soft HEAD`
    * Remove commits from current published branch (by creating a new commit, i.e., it does not re-write history): `git revert HEAD`
    * Interruptions to coding
        * Pulling into a dirty tree (i.e., git pull cannot merge): `git stash` then `git pull` and `git stash pop`
        * Interrupted workflow: `git stash`; work on interruption and commit; `git stash pop`

4. Commit to your development branch regularly and use explanatory commit messages in order to create a transparent work history (e.g., to help with debugging; to find specific changes at a later time). Each commit is a separate logical unit of change and is therefore composed of related changes.
    * Check state of staging area: `git status`
    * Commit/save to project history:
        * Commit staged snapshot: `git commit -m "<message>"` # where <message> contains the commit description on the first line (< 50 characters), a blank line, and a detailed description (include keywords to close/fix/resolve issues, e.g., closes #45 will close issue #45 in the repository)
    * Invoke a text editor to compose <message>: `git commit`
    * Commit all changes: `git commit -am "<message>"`

5. Share and backup your development commits on the remote repository. Our standard method for publishing local contributions to the github.com repository:
    0. Make sure you are on the development branch: `git checkout <branch>`
    1. Make local commit(s) to the development branch (see previous step)
    2. Import/merge new commits from remote/upstream to local repository branch: `git pull origin <branch>`
    3. Push/export local project history to remote/upstream: `git push origin <branch>`
Text tools, e.g., Winmerge/Textwrangler, compare two files (versions of the same file) and offer the possibility to take over changes from either version to the other version. Such functionality can be very helpful to resolve conflicts during merging.

6. Repeat steps 3-5 until the development branch is ready for deployment. Open a [pull request](https://help.github.com/articles/creating-a-pull-request/) and ask for feedback from the team members. It usually does not hurt if someone else than the developer does a test on a feature, since another person may test differently.

7. If necessary, repeat steps 3-5 to complete review of the pull request, e.g. to deal with issues and fix bugs.

8. After a team member has reviewed the development branch, you should deploy the development branch and merge/rebase to the master. Our standard method with two options for deploying a development/feature branch to the master branch on github.com repository (option (i) with rebasing is ideal for small development branches; option (ii) with merging is preferred for large development branches):
    0. Make sure you are on the development branch: `git checkout <branch>`
    1. Make sure the staging area is clean: `git status`
    	* Note: The Rsoilwat repository will always show 'src' as 'untracked content' because SOILWAT is loaded as a submodule. Don't stage and commit 'src'. If you want to edit the code of SOILWAT, do that in the SOILWAT repository and then update the submodule in Rsoilwat as explained in the README of Rsoilwat.
    2. If the repository is a R package, then adjust the lines 'Version' and 'Date' of the file DESCRIPTION to reflect the new version and potentially adjust package startup message in function '.onAttach' of the file R/zzz.R
    3. Option (i): Rebase development branch onto the tip of the master branch (given that there are no branches on <branch>): `git rebase master`
    4. Options (i) and (ii): Integrate with the main code base:
        1. `git checkout master`
        2. `git merge <branch>`
    5. Inspect outcome of merge (e.g., resolution of potential merge conflicts), commit and push the merge to remote/upstream with a detailed <message> particularly when using option (ii)
        1. `git commit -am "<message>"`
        2. `git push origin`
    6. Create an annotated version tag using semantic versioning with a format like v1.0.4
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
    7. Delete the development branch: `git branch -d <branch>`


## Links
* https://guides.github.com/introduction/flow/
* https://help.github.com/articles/closing-issues-via-commit-messages/
* http://clubmate.fi/git-dealing-with-branches-merging-and-rebasing/
* https://git-scm.com
    * https://git-scm.com/doc
    * https://git-scm.com/docs
    * https://git-scm.com/book
    * https://git-scm.com/book/en/v2/Git-Branching-Rebasing
* https://www.atlassian.com/git/tutorials
    * https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow
    * https://www.atlassian.com/git/tutorials/merging-vs-rebasing/workflow-walkthrough
