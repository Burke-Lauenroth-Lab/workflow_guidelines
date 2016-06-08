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

* __Testing__ involves at least that each line of code was executed and did not throw an error or stopped execution unexpectedly. However, writing re-usable unit test cases (for R code based on the package ‘testthat’, see, e.g., XXX) is the preferred way to test our code.

* __Documentation__: Comment the code well and write object documentation with [roxygen](http://r-pkgs.had.co.nz/man.html), if using R, or with [doxygen](http://www.doxygen.org), if using another programming language.

* We use __[semantic versioning](http://semver.org/)__ using the format MAJOR.MINOR.PATCH. Every commit to the master branch updates the version number. A backwards-incompatible commit increases MAJOR and resets MINOR and PATCH to 0. A backwards-compatible commit adding new functionality increases MINOR and resets PATCH to 0. A backward-compatible commit fixing bugs (etc) increases PATCH.

* __Inspecting__ a repository
    * State of working directory and staging area: git status
    * History of commits: `git log --graph --full-history --oneline --decorate`
    * List of branches (* indicates the active branch): 
    * Local branches: `git branch`
    * Remote branches: `git branch -r`
    * List of remote connections: `git remote -v`


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
    1. Create an annotated version tag using semantic versioning with a format like v1.0.4: `git tag -a v1.0.4 -m "<message>"`
    2. If the repository is a R package, then adjust the lines 'Version' and 'Date' of the file DESCRIPTION to reflect the new version and potentially adjust package startup message in function '.onAttach' of the file R/zzz.R
    3. Option (i): Rebase development branch onto the tip of the master branch (given that there are no branches on <branch>): `git rebase master`
    4. Options (i) and (ii): Integrate with the main code base:
```
git checkout master
git merge <branch>
```
    5. Inspect outcome of merge (e.g., resolution of potential merge conflicts), commit and push the merge to remote/upstream with a detailed <message> particularly when using option (ii)
```
git commit -a
git push origin --tags
```
    6. Add a [new release](https://help.github.com/articles/creating-releases/) against master based on the version tag
    
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



We haven't really published the code yet nor prepared it for sharing (though through our use of github made it openly accessible), it is actively and gradually being developed by the Schlaepfer lab, and there is no manual - we cannot give you individual support in setting up and running the code except if we agreed on a collaboration or similar agreement.

Not every part of the code has been extensively tested or is in a stable state. Similarly, not every combination of model inputs and options has been evaluated in depth and there is not guarantee for anything to work. The code comes with no warranty and no guarantees, expressed or implied, as to suitability, completeness, accuracy, and whatever other claim you would like to make.

There is no graphical user interface, help pages and available documentation may be out of date, and you will need to write your own tools to analyse outputs.

If you make use of this model, please cite appropriate references, and we would like to hear about your particular study (especially a copy of any published paper).


Some references of the implemented methods
* Danz, N.P., Frelich, L.E., Reich, P.B. & Niemi, G.J. (2012) Do vegetation boundaries display smooth or abrupt spatial transitions along environmental gradients? Evidence from the prairie–forest biome boundary of historic Minnesota, USA. Journal of Vegetation Science, 24, 1129-1140.
* Eppinga, M.B., Pucko, C.A., Baudena, M., Beckage, B. & Molofsky, J. (2013) A new method to infer vegetation boundary movement from 'snapshot' data. Ecography, 36, 622-635.
* Gastner, M., Oborny, B., Zimmermann, D.K. & Pruessner, G. (2009) Transition from connected to fragmented vegetation across an environmental gradient: scaling laws in ecotone geometry. The American Naturalist, 174, E23-E39.




### Install the package in R
```
devtools::install_git("https://github.com/dschlaep/ecotoner.git")
```

### Information about the package
```
package?ecotoner			# A few remarks
help(package = "ecotoner")	# The index page with some of the functions (which are documented to a varying degree)
```

### Using the package
I added the main code which I use to locate and measure my transects (producing the data for later analysis), as demo to the package. I also added data so that it can run  as a small contained demo 'example' (if the flag do.demo is set to TRUE). You could run this code directly with the demo() function, but this is probably not convenient:
```
demo("BSE-TF_ecotoner_LocateAndMeasure", package = "ecotoner")
```

Instead, the following command should open my main code in your text editor (at least on unix-alike systems) for easier inspection:
```
system2("open", file.path(system.file("demo", package = "ecotoner"), "BSE-TF_ecotoner_LocateAndMeasure.R"))
```


### For contributors only
### How to contribute
You can help us in different ways:

1. Reporting [issues](https://github.com/dschlaep/ecotoner/issues)
2. Contributing code and sending a [pull request](https://github.com/dschlaep/ecotoner/pulls)

In order to contribute to the code base of this project, you must first contact the Schlaepfer Lab. We retain any decision to accept your suggestions/contributions.
