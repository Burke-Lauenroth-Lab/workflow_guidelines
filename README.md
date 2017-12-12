# Structure, Workflow, and Standards for the Ecohydrology Laboratory GitHub Organization
------

* Version: Sep 16, 2016, modified December 11, 2017
* Authors: Alexander Reeder, Daniel Schlaepfer, Zachary Kramer, Caitlin Andrews

This is a general guide to the structure, workflow, and standards of the Ecohydrology Laboratory GitHub Organization. The Ecohydrology Laboratory GitHub Organization consists of many different repositories, each with a different code base and utility aimed at promoting out ability to model ecohydrology. We use the ['Github flow'](https://guides.github.com/introduction/flow/) (a 'feature branch workflow') as the basis for our projects.

## Table of Contents

[Basics](#basics)

[Suggested Tools and Software](#suggests)
  * [Text Editors](#text)
  * [Git GUIs](#gitgui)
  * [Merge](#merge)

[Communication](#communication)

[Repositories of the Ecohydrology GitHub Organization](#theRepos)
  * [SOILWAT2](#soilwat)
  * [rSOILWAT2](#rsoilwat)
  * [rSFSW2](#rsfsw)
  * [STEPWAT2](#stepwat)
  * [rSFSTEP2](#rsfstep)
  * [rSFSW2_tools](#sfsw_tools)
  * [STEPWAT_R_Wrapper](#stepr)

[Relationships between Repositories](#repoRelations)

[GitHub Features and Functionality](#useGitFeatures)
  * [Issues](#issues)
  * [Milestones](#milestone)
  * [Projects - Repository Level](#repoprojects)
  * [Projects - Organization Level](#organizationprojects)

[Workflow Guidelines](#workflow)

[Documentation Guidelines](#documentation)
  * [Documentation in C](#cdocumentation)
  * [Documentation in R](#rdocumentation)

[Testing Guidelines](#testing)
  * [Heirarchy of Testing](#testheir)
  * [Testing in C](#ctest)
  * [Testing in R](#rtest)
  
[Useful Links](#links)


## Basics <a name="basics"/>

* The 4 levels of the git organization
    * __Working directory__ (on local computer): work in text editor and/or an IDE (RStudio, Visual Studio, etc.)
    * __Staging area__: include/save changes to the next commit
    * __Local repository__: commit to project history
    * __Remote repository__ on github.com: share code with collaborators and backup local branches

* We use __issues__ and __milestones__ to communicate code enhancements, bugs, priorities, and current progress.

* We have __three types of branches__:
    * __Master branch__: Any commit on the master branch aims to be deployable. Such commits are usually the result of merging/rebasing with a feature or bugfix branch. Once deployable a unique version number is released. Deployable for us means that commits are tested.
    * __Bugfix branches__: When a bug is discovered on the master branch, a bugfix branch and an issue should be created. The issue should be assigned to the _master_ milestone. A bugfix branch should be named after the respective issue. An example of a bugfix branch would be bugfix_16, which represents issue #16.
    * __Feature branches__: Everything else should be a feature branch, which is where code development is done before it is merged back to master. Each feature branch needs its own __milestone__. Any bugfixes needed on a feature branch should be directly committed to the feature branch. Feature branches should have descriptive but concise names in upper camel case, such as feature_BetterErrorMessages.

* __Testing__ involves at least that each line of code was executed and did not throw an error or stopped execution unexpectedly. However, writing re-usable unit test cases (for R code based on the package [testthat](https://cran.r-project.org/web/packages/testthat/index.html), and for C code based on [GoogleTest](https://github.com/google/googletest)) is the preferred way to test our code.

* __Documentation__: Comment the code well and write object documentation with [roxygen2](http://r-pkgs.had.co.nz/man.html), if using R, or with [doxygen](http://www.doxygen.org), if using another programming language.

* __Style guide__: In development. For R coding style, DRS suggests to follow [Hadley Wickham's style guide for R](http://adv-r.had.co.nz/Style.html). There are many other R style guides including: [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml), [Bioconductor's Coding Style](https://www.bioconductor.org/developers/how-to/coding-style/), [rDatSci/rOpenSci's R Style Guide](https://github.com/rdatsci/PackagesInfo/wiki/R-Style-Guide), [R Coding Conventions by H. Bengtsson](https://docs.google.com/document/d/1esDVxyWvH8AsX-VJa-8oqWaHLs4stGlIbk8kLc5VlII/edit), [4D R code style guide](https://4dpiecharts.com/r-code-style-guide/)

* We use __[semantic versioning](http://semver.org/)__ using the format MAJOR.MINOR.PATCH. Every commit to the master branch updates the version number. A backwards-incompatible commit increases MAJOR and resets MINOR and PATCH to 0. A backwards-compatible commit adding new functionality increases MINOR and resets PATCH to 0. A backward-compatible commit fixing bugs (etc) increases PATCH.

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

* __Error reporting__:
    * All _master branch_ bugs require you to create an __issue__ in GitHub. Please think carefully whether the issue is due to code by SOILWAT2, rSOILWAT2, or rather by rSFSW2. Ideally, you provide a unit tests which demonstrates the failing code. This unit test serves also as a benchmark to identify the solution of the issue. If it is not possible to write a unit test, please provide a minimal reproducible example. Some resources that may help:
        * [How to create a Minimal, Complete, and Verifiable example](http://stackoverflow.com/help/mcve)
        * [How to Report Bugs Effectively](http://www.chiark.greenend.org.uk/~sgtatham/bugs.html)
        * [How to make a great R reproducible example?](http://stackoverflow.com/questions/5963269/how-to-make-a-great-r-reproducible-example#5963610)
        * [How to write a reproducible example](https://gist.github.com/hadley/270442)
    * Assign the issue to the __master__ milestone
    * Create a __bugfix branch__
    * [Close the issue with a reference](#closing-issues)
    * Create a pull request to the master branch, with appropriate reviewers

## Suggested Tools and Software <a name="suggests"/>

### Text Editors  <a name="text"/>

### Git GUIs  <a name="gitgui"/>

Git GUIs provide an accessible interface to the Git framework. Users are able to quickly track and pull the latest commits by other users, add and push their own commits.
	* ['GitHub Desktop'](https://desktop.github.com/) for Mac OSX and Microsoft Windows allows for visually friendly handling of pull requests, merges, commits, branches, and diffs, but lacks some advanced features (e.g., management of sub-modules). 
	*['Sourcetree'](https://www.sourcetreeapp.com/), provides similar functionality to 'Github Desktop', but covers most of the advanced features that it lacks.

### Merge <a name="merge"/>



## Communication (commit messages, comments on issues, etc.) <a name="communication"/>

Follow our code of conduct in all communications, e.g., [Contributor Code of Conduct of the rSFSW2 repository](https://github.com/Burke-Lauenroth-Lab/rSFSW2/blob/master/CONDUCT.md).

Why good messages are important:
[Erlang: Writing good commit messages](https://github.com/erlang/otp/wiki/Writing-good-commit-messages): "Good commit messages serve at least three important purposes:
- To speed up the reviewing process.
- To help us write a good release note.
- To help the future maintainers of Erlang/OTP (it could be you!), say five years into the future, to find out why a particular change was made to the code or why a specific feature was added."

How to write good messages:
[Who-T: On commit messages](http://who-t.blogspot.com/2009/12/on-commit-messages.html): "A good commit message should answer three questions about a patch:
- Why is it necessary? It may fix a bug, it may add a feature, it may improve performance, reliabilty, stability, or just be a change for the sake of correctness.
- How does it address the issue? For short obvious patches this part can be omitted, but it should be a high level description of what the approach was.
- What effects does the patch have? (In addition to the obvious ones, this may include benchmarks, side effects, etc.)"


## Repositories of the Ecohydrology GitHub Organization <a name="theRepos"/>

### SOILWAT2 <a name="soilwat">
	
### rSOILWAT2 <a name="rsoilwat">

### rSFSW2 <a name="rsfsw">

### STEPWAT2 <a name="stepwat">

### rSFSWTEP2 <a name="rsfstep">

### rSFSW2_tools <a name="sfsw_tools">

### STEPWAT_R_Wrapper <a name="stepr">


## Relationships between XXXX Lab's Repositories <a name="repoRelations"/>

## GitHub Features and Functionality <a name="useGitFeatures"/>

### Issues <a name="issues"/>

Issues are the smallest unit of reporting and are repository specific. Issues can pertain to tasks, questions, proposed enhancements, and bugs. For bug fixes, the issue should revolve around a containable unit of code. Additionally, for each bug, a __bugfix__Issue#BugDescription__ branch should be created. 

#### Creating issues 
If you create an issue, decide to work on one, or you are assigned to one, then:
* Assign it to a team member and/or yourself, if applicable
* Add an __in progress__ label, if you are currently working on it
* Add a __priority__ label, if it has a low priority or a high priority
* Add a __category__ label, such as 'bug' or 'enhancement' or 'question'
* Assign it to a milestone
    * Master branch bug: the _master_ milestone
    * Feature branch bug/enhancement: the respective milestone

#### Closing issues 
* When the issue is resolved, [reference it](https://help.github.com/articles/closing-issues-using-keywords/) in the final commit that solves it (which can be done in the title or body). 
	* For example, including 'Fixes #45' or 'Closes #45' in the commit message will close issue #45 in the repository you are working in.
	* Note: Issues will not be closed via reference until the branch is merged to master. If you are resolving an issue on a feature branch, please still use a reference, as it provides beneficial documention, but you should also manually close the issue afterwards.

### Milestones <a name="milestone"/>
Milestones map to branches within a repository, and hold issues relating to that branch. Whenever you create a _feature_ branch, you should also create a respective milestone.
* Name it the exact same as the branch name, minus the "feature_" prefix
    * For instance, _feature_CO2Effects_ should be named _CO2Effects_
* Provide an apt description of the branch
* Optionally, provide a deadline

### Projects - Repository Level <a name="repoprojects"/>
Projects on GitHub are meant to assist in the organization and prioritization of the implementation of broad and/or future ideas. Each project consists of  three different columns: To Do, In Progress, and Done. In each of these columns, the user can broach different tasks and ideas that are free-from or that directly reference existing issues.

### Projects - Organization Level <a name="organizationprojects"/>
Projects at the organizational level are similar to those at the repository level, except that they can span multiple repositories. This is useful for the implementation of features that have, for example, both a C (e.g. SOILWAT2) and R (e.g. rSOILWAT2) component.

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

## Documentation Guidelines <a name="documentation"/>

### Documentation in C <a name="cdocumentation"/>

### Documentation in R <a name="rdocumentation"/>

  
## Testing Guidelines <a name="testing"/>

Software testing is the process of executing a series of tests at different levels to detect bugs. It it the process of validating and veryifying that our programs are acting as expected as designed.

### Heirarchy of Testing <a name="testheir"/>

We test at a series of different _levels_. From lowest to highest, these are:

* __Unit Tests:__ Unit tests test the smallest testable part of a software. We write unit tests around different __functions__, checking that the output is as expected, based on function inputs and arguments. These tests are completely user-generated, and force us to design and write good code. Unit tests also allow for the easy and pointed detection of an error in the code, as we make changes and add functionality.

* __R Package Tests:__ As the name suggests, these tests are for R packages only (i.e. rSFSW2 and rSOILWAT2). R package tests execute all unit tests for that package, as well as a series of other checks aimed at detecting common problems. [More information here.](http://r-pkgs.had.co.nz/check.html)

* __Continuous Integration & GitHub Checks:__ GitHub checks are automated so that feature or bugfix branches cannot be merged/ a pull request cannot be approved until these checks are passed. Continous integration (CI) is the automation of building, testing, and validating code as new commits are made, across platforms. CI, as the name implies, occurs continually so that the source of error or conflict are more easily tracked. The master branch is always kept clean, and CI checks are tested as pull requests are submitted. Our CI GitHub checks currently consist of coverage checks and platform build checks. 
	* Coverage checks look to see that the percentage of line of code covered by unit tests has _increased_. If it hasn't the test will not pass.
	* Platform build checks are tested on a Unix and Windows Servers using Travis and Appveyor, respectively. Travis and Appveyor are servers with these OSs where the program is built and checked. This is useful because if, for example, SOILWAT2 is not building on an individual's Linux computer, but is building on Travis, we know the user's computer is not configured correctly, as opposed to a software design issue.
		
* __Test Projects:__ 

### Unit Testing in C <a name="ctest"/>

### Unit Testing in R <a name="rtest"/>


## Links <a name="links"/>

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

