# Structure, Workflow, and Standards for the Dryland Ecology Laboratory GitHub Organization
------

* Version: Sep 16, 2016, modified December 11, 2017
* Authors: Alexander Reeder, Daniel Schlaepfer, Zachary Kramer, Caitlin Andrews

This is a general guide to the structure, workflow, and standards of the Dryland Ecology Laboratory GitHub Organization. The Dryland Ecology Laboratory GitHub Organization consists of many different repositories, each with a different code base and utility aimed at promoting out ability to model ecohydrology. We use the ['Github flow'](https://guides.github.com/introduction/flow/) (a 'feature branch workflow') as the basis for our projects.

## Table of Contents

[Repositories of the Dryland Ecology GitHub Organization](#theRepos)
  * [SOILWAT2](#soilwat)
  * [rSOILWAT2](#rsoilwat)
  * [rSFSW2](#rsfsw)
  * [STEPWAT2](#stepwat)
  * [rSFSTEP2](#rsfstep)
  * [rSFSW2_tools](#sfsw_tools)
  * [STEPWAT_R_Wrapper](#stepr)

[Relationships between Repositories](#repoRelations)

[Suggested Tools and Software](#suggests)
  * [Text Editors](#text)
  * [IDEs](#ides)
  * [Git GUIs](#gitgui)
  * [Merge](#merge)
  
[GitHub Basics](#basics)
 
[Coding Style and Practices](#coding)
  * [R Code Style](#rcode)
  * [C Code Style](#ccode)
  * [Best Coding Practices](#codepractice)

[Communication](#communication)

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

## Repositories of the Dryland Ecology GitHub Organization <a name="theRepos"/>

### SOILWAT2 <a name="soilwat">

The SOILWAT2 repository contains the __C__ code, as well as the testing and documentation, pertaining to the SOILWAT2 model.  SOILWAT2 is a site-specific, daily ecohydrology model that tracks water as it moves through the environment. Within the C code you will find the series of equations that translates our knowledge of ecohydrology to a simulation framework. This model is the foundation of our research activities and most other repositories within this organization connect or interact with this model in some way.

SOILWAT2 code is reserved to handle the ecological assumptions of how water moves through environment, based on a series of inputs. Inputs, from a SOILWAT2 perspective, are static. The one excpetion to this is vegetation _if_ the carbon dioxide effects options are set to on. The SOILWAT2 model is designed to function 'stand-alone', using files.in as inputs, to work with information passed via rSOILWAT2, or to work with information passed from STEPWAT2.

### rSOILWAT2 <a name="rsoilwat">

The rSOILWAT2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSOILWAT2 package. The primary functions of rSOILWAT2 are to: (1) Pass site-specific information from the wrapper (rSFSW2) to the SOILWAT2 model; And (2) to create a SQLite weather database for simulation runs. In the the future item (2) will be separated into its own R package. Currently, the STEPWAT2 model is accessing the rSOILWAT2 package for its weather database functionality.

### rSFSW2 <a name="rsfsw">

The rSFSW2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSFSW2 package. The primary functions of the rSFSW2 packages are to: (1) Handle input data from multiple sites efficiently; (2) Grab additional inputs from stored or online resources; (3) Calculate additional inputs based on user treatment and experimental design options; (4) Pass other treatment and experimental options to the SOILWAT2 model to use in executions; and (5) Receive and aggregate output from SOILWAT2. 

### STEPWAT2 <a name="stepwat">

The STEPWAT2 repository contains the __C++__ code, as well as the testing and documentation, pertaining to the STEPWAT2 model. 

### rSFSTEP2 <a name="rsfstep">

The rSFSTEP2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSFSTEP2 model. 
It interfaces with the STEPWAT2 C code and runs in parallel for multiple sites, climate scenarios, disturbance regimes, and time periods.

### rSFSW2_tools <a name="sfsw_tools">

The rSFSW2 tools repostory contains functionality that assist in developing and testing the rSFSW2 package.

## Relationships between Repositories <a name="repoRelations"/>

SOILWAT2 runs on a site by site basis. While the simulations of water balance are fast, running SOILWAT2 stand-alone is inefficient, as it needs to read and write information from disk about each site individually. Furthermore, running simulations for many sites is cumbersome from the user perspective, as a series of site-specific input files (.in) need to be formatted for each site. Because of these issues, the R Soilwat Wrapper (rSFSW2) and the R Soilwat Package (rSOILWAT2) were developed. These repositories allow users to set-up information for multiple sites simultaneously and for the efficient handling of these sites' information in R. 

rSOILWAT2 is an R package with a series of functions that passes information as a S4 class object between R and C. rSOILWAT2 knows nothing about your site or water-balance; It is simply a package with functions designed to interface between the two programs. rSOILWAT2 avoids disk operations and most things happen in memory. Information, read-in, or calculated, and formatted by rSFSW2, is passed via rSOILWAT2 to SOILWAT2. Most importantly, it allows for the execution of SOILWAT2 to happen completely in memory.

rSFSW2 is designed to gather user's options and information about sites. All site-specific inputs and treatments in a project are read in at once via a series of _.csvs_. The information within these .csvs is then parsed up on a site by site basis, processed, and then sent to SOILWAT2 via the functions in rSOILWAT2. Outputs from the SOILWAT2 model are then sent back to rSFSW2 via rSOILWAT2 and either saved on a site by site basis, or aggregating and stored in a SQLite outputs databased.


## Suggested Tools and Software <a name="suggests"/>

A wide array of tools and software is available to help with the ease of managing code and workflow across multiple repositories with many collaborators. The options below remain merely as suggestions.

### Text Editors  <a name="text"/>

There are two types of text editors: (1) Those accessed via the command line and (2) editors with GUIs.

__Command line editors__ are typically used for quick edits in the terminal. Options include nano, vim, and vi. I find nano easiest to use. Most all shortcuts are listed at the bottom of the window, there is autotomatic indentation, and many search functions. Easy to use and master.

 * `git config --global core.editor <editor>` 
 
__GUI text editors__ are typically more user-friendly, particuarly when approaching coding projects with multiple interacting files. Multiple projects can be opened and stored. Atom and Sublime text are two of the most popular. Atom is completely 'hackable' meaning anyone can write code to improve the source code. 

### Git GUIs  <a name="gitgui"/>

Git GUIs provide an accessible interface to the Git framework. Users are able to quickly track and pull the latest commits by other users, add and push their own commits.
	* ['GitHub Desktop'](https://desktop.github.com/) for Mac OSX and Microsoft Windows allows for visually friendly handling of pull requests, merges, commits, branches, and diffs, but lacks some advanced features (e.g., management of sub-modules). 
	*['Sourcetree'](https://www.sourcetreeapp.com/), provides similar functionality to 'Github Desktop', but covers most of the advanced features that it lacks.

### Merge <a name="merge"/>

Merge tools allow ease of access when integrating pull requests and merges into other branches. Options include Kdiff3, meld, and vimdiff. Kdiff3 is completely free to use, and very user-friendly. Conflicts are show line by line, character by character, and provides an automated way to merge differenves.

 * `git config --global merge.conflictstyle diff3` # conflict resolution with three sections: HEAD (code between `<<<<<<<` and `|||||||`), feature-branch  (code between `=======` and `>>>>>>>`), and (3rd) merged (=last) common ancestor (code between `|||||||` and `=======`)

## GitHub Basics <a name="basics"/>

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
    
## Coding Style and Practices <a name=#coding"/>

  ### [R Code Style] <a name="rcode"/>
  * __Style guide__: In development. For R coding style, DRS suggests to follow [Hadley Wickham's style guide for R](http://adv-r.had.co.nz/Style.html). There are many other R style guides including: [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml), [Bioconductor's Coding Style](https://www.bioconductor.org/developers/how-to/coding-style/), [rDatSci/rOpenSci's R Style Guide](https://github.com/rdatsci/PackagesInfo/wiki/R-Style-Guide), [R Coding Conventions by H. Bengtsson](https://docs.google.com/document/d/1esDVxyWvH8AsX-VJa-8oqWaHLs4stGlIbk8kLc5VlII/edit), [4D R code style guide](https://4dpiecharts.com/r-code-style-guide/)

  ### [C Code Style] <a name="ccode"/>
  ### [Best Coding Practices]<a name="codepractice"/>
    
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

We execute a series of tests at different levels to detect bugs and to validate and verify that our code is acting as expected.

### Heirarchy of Testing <a name="testheir"/>

We test at a series of different _levels_. From lowest to highest, these are:

* __Unit Tests:__ Unit tests test the smallest testable part of a software. We write unit tests around different __functions__, checking that the output is as expected, based on function inputs and arguments. These tests are completely user-generated, and force us to design and write good code. Unit tests also allow for the easy and pointed detection of an error in the code, as we make changes and add functionality.

* __R Package Tests:__ As the name suggests, these tests are for R packages only (i.e. rSFSW2 and rSOILWAT2). R package tests execute all unit tests for that package, as well as a series of other checks aimed at detecting common problems. [More information here.](http://r-pkgs.had.co.nz/check.html)

* __Continuous Integration & GitHub Checks:__ GitHub checks are automated so that feature or bugfix branches cannot be merged / a pull request cannot be approved until these checks are passed. Continous integration (CI) is the automation of building, testing, and validating code as new commits are made, across platforms. CI, as the name implies, occurs continually so that the source of error or conflict are more easily tracked. The master branch is always kept clean, and CI checks are tested as pull requests are submitted. Our CI GitHub checks currently consist of coverage checks and platform build checks. 
	* Coverage checks look to see that the percentage of line of code covered by unit tests has _increased_. If it hasn't the test will not pass.
	* Platform build checks are tested on a Unix and Windows Servers using Travis and Appveyor, respectively. Travis and Appveyor are servers with these OSs where the program is built and checked. This is useful because if, for example, SOILWAT2 is not building on an individual's Linux computer, but is building on Travis, we know the user's computer is not configured correctly, as opposed to a software design issue.
		
* __Test Projects:__ Testing at the comprehensive level to test that new code or feature is still producing the same or sensible output. Additionally, test projects provide an example of how the user should set up a project for it to function properly. Checks are made on speed and against reference databases.

### Unit Testing in C <a name="ctest"/>

### Unit Testing in R <a name="rtest"/>

We use the 'testthat' framework for unit testing in R. Within each of our R package repositories there is a _tests/testthat_ directory. Within the _tests_ folder there is testthat.R, which guides R to to test all files in the _testthat_ folder during the R CMD check. Each file within the _/testthat_ folder should begin with __test__ and each of these files contains multiple related tests. We typically have _at least_ a test file for each corresponding file in the /R folder, but it is possible that there might need to be many test files for each /R file.

Each test should be designed to test the expectation or multiple expectations of a function. Is the output the right value or the right class? Are the values equal to a reference value? Is there an error message, warning, or message when we want there to be?  There is more information on the variety of available test functions and how to use theme [here.](http://r-pkgs.had.co.nz/tests.html#test-tests)

Each and every function should have a test. Our goal is 100% code coverage. As you write tests, check that they are working by with Ctrl/Cmd-Shift-T or devtools::tests().

#### An Example




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

