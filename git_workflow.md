# Git and GitHub Workflow Guidelines

Our workflow consists of 11 basic steps:

- 1. [Set Configurations](#1)
- 2. [Clone Repository](#2)
- 3. [Create Issue and/or Milestone](#3)
- 4. [Create Branch](#4)
- 5. [Develop Code](#5)
- 6. [Commit Code](#6)
- 7. [Merge Master](#7)
- 8. [Open Pull Request](#8)
- 9. [Pass Continuous Integration](#9)
- 10. [Merge Development Into Master](#10)
- 11. [Update Version](#11)

Note: All code presented in this document are git commands from terminal. Many alternatives exist with Git GUIs or on GitHub.com.

## 1.	Set Configurations <a name="1"/>

For first time users:
* Download necessary programs
  * [Downloads](#README.md dls)

* Set __global user options__ to identify your commits
  * `git config --global user.name <name>`
  * `git config --global user.email <email>`
  * `git config --global core.editor <editor>` # e.g., vi, emacs, nano, TextWrangler (Microsoft Windows user refer to [First-Time-Git-Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup))
  * `git config --global merge.conflictstyle meld` # see merge section

- Activate two-factor authentication for your account on [github.com](https://help.github.com/articles/providing-your-2fa-authentication-code/)
  * DRS prefers using a TOTP application over text messaging, e.g., [Duo Mobile](https://duo.com/solutions/features/two-factor-authentication-methods).
  * Command line tools will require a [personal access token](https://help.github.com/articles/creating-an-access-token-for-command-line-use/) instead of your regular password.
  * Instead of repeatedly entering the token, you could enable 'git credential caching' with `git config --global credential.helper cache` (on Linux) or `git config --global credential.helper osxkeychain` (on macOS).

## 2. Clone Repository <a name="2"/>

Repositories hosted on our GitHub organization should be cloned into a local 'Git' folder on your desktop with the git clone command. Link your Git Desktop and text editor (i.e. Atom) to these folders.
    * `git clone https://github.com/DrylandEcology/SOILWAT2`

## 3. Create Issue and/or Milestone <a name="3"/>

Issues are created via GitHub.com. Issues are the smallest unit of reporting and are repository specific. Issues can pertain to tasks, questions, proposed enhancements, and bugs. For bug fixes, the issue should revolve around a containable unit of code. Each issue should be assigned to a milestone.

### Creating Issues

If you create an issue, decide to work on one, or you are assigned to one, then:
* Assign it to a team member and/or yourself, if applicable
* Assign it to a milestone
    * _ToDo_ milestone: A rolling, catch-all milestone for bugfixes or issues that will not be immediately addressed.
    * _Enhancement_ milestones: Pertains to the specific development of a feature or enhancement and most relates a specific branch.
* Add an __in progress__ label, if you are currently working on it
* Add a __priority__ label, if it has a low priority or a high priority
* Add a __category__ label, such as 'bug' or 'enhancement' or 'question'

#### Bugs and Error Reporting

If your issue is a bug then:
  * Provide a unit tests which demonstrates the failing code. This unit test serves also as a benchmark to identify the solution of the issue. If it is not possible to write a unit test, please provide a minimal reproducible example and a screenshot of the error message you are receiving. Some resources that may help:
      * [How to create a Minimal, Complete, and Verifiable example](http://stackoverflow.com/help/mcve)
      * [How to Report Bugs Effectively](http://www.chiark.greenend.org.uk/~sgtatham/bugs.html)
      * [How to make a great R reproducible example?](http://stackoverflow.com/questions/5963269/how-to-make-a-great-r-reproducible-example#5963610)
      * [How to write a reproducible example](https://gist.github.com/hadley/270442)
  * Assign the issue to the __ToDO__ milestone

### Closing Issues

When a issue is resolved, [reference it](https://help.github.com/articles/closing-issues-using-keywords/) in the final commit that solves it (which can be done in the title or body).
  * For example, including 'Fixes #45' or 'Closes #45' in the commit message will close issue #45 in the repository you are working in.
  * Note: Issues will not be closed via reference until the branch is merged to master. If you are resolving an issue on a feature branch, please still use a reference, as it provides beneficial documentation, but you should also manually close the issue afterwards.


## 4. Create Branch <a name="4"/>

Create a branch to work on a bugfix or feature.
  * Make a new branch and check it out: `git checkout -b <branch>`
  * Push/export local <branch> to remote/upstream: `git push origin <branch>`

 We have __three types of branches__:
  * __Master branch__: Any commit on the master branch aims to be deployable. Such commits are usually the result of merging/rebasing with a feature or bugfix branch. Once deployable a unique version number is released. Deployable for us means that commits are tested.
  * __Bugfix branches__: When a bug is discovered on the master branch, a bugfix branch and an issue should be created. The issue should be assigned to the _master_ milestone. A bugfix branch should be named after the respective issue. An example of a bugfix branch would be bugfix_16, which represents issue #16.
  * __Feature branches__: Everything else should be a feature branch, which is where code development is done before it is merged back to master. Each feature branch needs its own __milestone__. Any bugfixes needed on a feature branch should be directly committed to the feature branch. Feature branches should have descriptive but concise names in upper camel case, such as feature_BetterErrorMessages.

## 5. Develop and Document Code <a name="5"/>

See [pertinent developing code](README.md) protocols to maintain code integrity.

A good rule of thumb is that other group members should know what you are working on at any given point in time. You should aim to document every significant change that will need to be done before the branch can be merged to master. Documentation should occur both within the code (both formal and notes), as well as within a Project if your code is a enhancement.

## 6. Stage and Commit Code <a name="6"/>

"A commit should be a wrapper for related changes." Read these [GitHub best commit practices](https://github.com/trein/dev-best-practices/wiki/Git-Commit-Best-Practices) that we follow.

In general, you can stage multiple changes to save and track your work, but a commit should only be pushed to GitHub when you are done working on a logical chunk of code. This is so the code can easily be rolled back. This doesn't mean your changes are huge, but rather that you commit based on the completion of a fix or enhancement into logical chunks. For example, if your mission is to write unit tests for all functions in one .R file, you could have a commit after you complete tests for one or two functions.

### Good Commit Messages

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

Rules for commit messages:
- Begin message with a short summary (< 50 character)
- Press enter
- Have bullet points that outline specifics including reasoning for specific changes

An example of a good commit message [commit 0d6577](https://github.com/DrylandEcology/rSFSW2/pull/306/commits/14c6fe45729fb288cb5ec5438b51d80db60d6577):

`Prepare, use, and populate the temporary output databases

- function `run_simulation_experiment` is now calling
`make_temporary_dbOutputs` before running the simulation runs

- function `move_output_to_dbOutput` gained a new argument
`use_dbTempOut` that is by default set to true`

### Stage and Commit Workflow:

* __Stage__ your changes in a snapshot: `git add <file>` or `git add <directory>` or `git add -p`
  * Note: Several GUIs allow staging per line(s) instead of per file. Good for organizing different aspects of development into logical commit units.
* __Commit__ to your development branch regularly and use explanatory commit messages in order to create a transparent work history (e.g., to help with debugging; to find specific changes at a later time).
   * Check state of staging area: `git status`
   * Commit/save to project history
   * Invoke a text editor to compose message: `git commit`
   * _Strongly_ follow [these guidelines](https://github.com/erlang/otp/wiki/Writing-good-commit-messages) for writing a message
   * Commit all changes: `git commit -am "<message>"`
* Make sure the staging area is clean: `git status`
* In case someone else is working on the same development branch, then import/merge new commits from remote/upstream to local branch: `git pull origin <branch>`
* Push/export local project history to remote/upstream: `git push origin <branch>`


## 7. Merge Master <a name="7"/>

Merging is the act of integrating the changes of one branch into another. Typically, you are merging changes on the _master_ branch into your development branch, though any branch can be be merged into another. Incorrect merging is the most frequent source of error and can have serious consequences, so make sure to be conscientious about the merge decisions you are making.

### Merge Tips

The most effective way to correctly merge is to be __proactive and conscientious__ before the merging occurs. Use a Git GUI to inspect (1) what changes have occurred on the master branch and (2) what changes have occurred on your development branch. If separate files have been worked on and there is no overlap in file development, your merge will have no conflicts and be straightforward. In fact, when you open a pull request on GitHub, it will automatically detect whether your development branch has conflicts or not. If there is none, there is an option to automatically merge.

However, if there are merge conflicts (i.e. there has been development on the same file & lines on both branches), you will need inspect these conflicts, one by one, and decide which version is to be included in the final merged version of your branch. Make sure to read the commit messages from the recent changes in the master to understand why and where there has been changes in code. In general, you should aim to retain the specific functionality (whether it was an enhancement or bugfix) on your branch, but make sure it integrates well with the, potentially new, overall workflow of the master. There are many situations where you may need to go back and re-write functionality in your feature branch based on new changes in the master, after the merge.

### Merge Tools

We require that our developers use either Atom or kdiff3 software to assist with clean and accurate merging. We also require that the merge conflict style is [diff3](https://stackoverflow.com/questions/27417656/should-diff3-be-default-conflictstyle-on-git), which can be set with:
`git config --global merge.conflictstyle diff3`

In Atom:

- In terminal or on GitHub, merge master into the development branch
  * On terminal: `git merge master` while checked out to development branch
- A message will appear indicating whether the master was successfully merged (no conflicts) or if there were conflicts
  * On terminal: `Automatic merge failed; fix conflicts and then commit the results`
- Open Atom and fix conflicts
  * Watch this [tutorial](https://atom.io/packages/merge-conflicts) on using Atom to resolve merge conflicts
  * Manually choose, conflict by conflict, whether you want the Master version, your development version ("Head"), or a combination of both
- Test that your development branch still works and passes all tests
- Commit changes that took place during merge
  * `git commit -am "<message>"`

In [kdiff3](http://kdiff3.sourceforge.net/):

Kdiff3 is a 3-way, 4-view merge tool, allowing the user to explicitly see the base, development, master, and merged differences.

- Download [kdiff3](http://kdiff3.sourceforge.net)
- Set kdiff3 as your merge tool
  * `git config --global merge.tool kdiff3`
- In terminal or on GitHub, merge master into the development branch
  *  `git merge master` while checked out to development branch
- A message will appear indicating whether the master was successfully merged (no conflicts) or if there were conflicts
  * `Automatic merge failed; fix conflicts and then commit the results`
- Open kdiff3
  * `git mergetool`
- Four windows will be shown
  * The top left window is the base (original), the top middle window is the development branch, the top right is the master, and the bottom pane is the end result of merging.
- Commit changes that took place during merge
  * `git commit -am "<message>"`


Note: If the tool is not one of the defaults with git, a path needs to be added so git can find and run the desired program. For example, `git config --global mergetool.meld.path c:\program Files (x86)\Meld\meld\meld.exe'`

Useful links:
  * [git-merge](https://git-scm.com/docs/git-merge)
  * [git how to](https://githowto.com/resolving_conflicts)

## 8. Open Pull Request <a name="8"/>

On GitHub.com, in the relevant repository, navigate to the 'Pull Request' tab. Open a [pull request](https://help.github.com/articles/creating-a-pull-request/) for your branch. Write a summary of the feature or fixes of your branch, and reference the issue #, so that the issues will be closed when your PR is closed.

Tag your supervisor for code review.

If necessary, repeat steps 5-7 to complete review of the pull request, e.g. to deal with issues and fix bugs.

## 9. Pass Continuous Integration Checks <a name="9"/>

When you open a pull request, your branch will be automatically tested to see if it passes our continuous integration (CI) tests. Your branch cannot be merged into master until these tests are passed.

(CI) is the automation of building, testing, and validating code as new commits are made, across platforms. CI, as the name implies, occurs continually so that the source of error or conflict are more easily tracked. The master branch is always kept clean, and CI checks are tested as pull requests are submitted. Our CI GitHub checks currently consist of coverage checks and platform build checks.
  * Coverage checks look to see that the percentage of line of code covered by unit tests has _increased_. If it hasn't the test will not pass.
  * Platform build checks are tested on a Unix and Windows Servers using Travis and Appveyor, respectively. Travis and Appveyor are servers with these OSs where the program is compiled, installed, and all unit tests are run. This is useful because if, for example, SOILWAT2 is not building on an individual's Linux computer, but is building on Travis, we know the user's computer is not configured correctly, as opposed to a software design issue.

## 10. Merge Development Into Master <a name="10"/>

Follow the steps above for (6). Should be straightforward and with minimal conflicts at this point.

You can now delete the development branch.
* Delete the local branch: `git branch -d <branch>`
* Delete the remote branch: `git push origin --delete <branch>`
* Remove 'obsolete tracking branches', i.e., branches on local machine that no longer exist on remote/github: `git fetch --all --prune`

## 11. Update Version <a name="11"/>

To update the version and draft a new release navigate to the 'release' tab in the repository. Click 'Draft a new release'
and update the version number and document the changes to the code with this release

We use __[semantic versioning](http://semver.org/)__ using the format MAJOR.MINOR.PATCH for our version numbering. Every commit to the master branch updates the version number. A backwards-incompatible commit increases MAJOR and resets MINOR and PATCH to 0. A backwards-compatible commit adding new functionality increases MINOR and resets PATCH to 0. A backward-compatible commit fixing bugs (etc) increases PATCH.
* PATCH update: Bug fix, minor enhancement, addition of unit tests and documentation.
* MINOR update: Substantial enhancement.
* MAJOR update: Large change to core functionality and design. Rare.
