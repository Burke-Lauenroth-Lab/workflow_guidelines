# Structure, Workflow, and Standards for the Dryland Ecology Laboratory GitHub Organization
------

* Authors:  Caitlin Andrews, Daniel Schlaepfer & Zachary Kramer

This is the Dryland Ecology Laboratory's protocol and guide to the structure, workflow, and standards of the Dryland Ecology Laboratory GitHub Organization. The Dryland Ecology Laboratory GitHub Organization consists of many different repositories, each with a different code base and utility aimed at promoting out ability to model ecohydrology.

This document is provided to orient developers to the purpose and relationships between our repositories as well as clearly explain our expectations in (A) using Git and GitHub and (B) developing code, including unit testing and documentation. These are our broad standards that apply to all of our repositories and the languages we develop in (C, C++, R, make). We rely on many other styles guides, software, etc. to assist in our workflow. This document provides expectation on what tools to use, but not specifics, as that is covered thoroughly by the creators of these tools.

More information on repository specific code development, testing (both informal and formal), and installation can be found in the README.md document found in _each_ repository. Per GitHub's functionality, the README document can be viewed by scrolling towards the bottom of each repository's homepage.


## Table of Contents

[Repositories of the Dryland Ecology GitHub Organization](#theRepos)
  * [SOILWAT2](#soilwat)
  * [rSOILWAT2](#rsoilwat)
  * [rSFSW2](#rsfsw)
  * [STEPWAT2](#stepwat)
  * [rSFSTEP2](#rsfstep)
  * [rSFSW2_tools](#sfsw_tools)
  * [STEPWAT_R_Wrapper](#stepr)
  * [Additional Repositories](#addrepos)

[Relationships between Repositories](#repoRelations)

[Git and GitHub](#gits)
  * [Interacting with Git](#interacting)
  * [Developing Code with Git](#developing)
  * [GitHub Communication Features](#gitcomm)
  * [GitHub Code Quality Features](#gitqual)

[Developing Code](#develop)

  * [Heirarchy of Testing](#testheir)

  * [R Code Standards](#rinfo)
    * [Style Guide](#rstyle)
    * [Code Development](#rdevel)
    * [Documentation](#rdoc)
    * [Unit Testing](#rtest)

  * [C Code Standards](#cinfo)
    * [Style Guide](#cstyle)
    * [Code Development](#cdevel)
    * [Documentation](#cdoc)
    * [Unit Testing](#ctest)

[Downloads](#dls)
[Useful Links](#links)

## Repositories of the Dryland Ecology GitHub Organization <a name="theRepos"/>

### SOILWAT2 <a name="soilwat">

The SOILWAT2 repository contains the __C__ code, as well as the testing and documentation, pertaining to the SOILWAT2 model.  SOILWAT2 is a site-specific, daily ecohydrology model that tracks water as it moves through the environment. Within the C code you will find the series of equations that translates our knowledge of ecohydrology to a simulation framework. This model is the foundation of our research activities and most other repositories within this organization connect or interact with this model in some way.

SOILWAT2 code is reserved to handle the ecological assumptions of how water moves through environment, based on a series of inputs. Inputs, from a SOILWAT2 perspective, are static. The one exception to this is vegetation _if_ the carbon dioxide effects options are set to on. The SOILWAT2 model is designed to function 'stand-alone', using files.in as inputs, to work with information passed via rSOILWAT2, or to work with information passed to or from STEPWAT2.

Specifics about downloading, installing, and using SOILWAT2 can be found in the [SOILWAT2 README](https://github.com/DrylandEcology/SOILWAT2/blob/master/README.md).

### rSOILWAT2 <a name="rsoilwat">

The rSOILWAT2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSOILWAT2 package. The primary functions of rSOILWAT2 are to: (1) Pass site-specific information from the wrapper (rSFSW2) to the SOILWAT2 model; (2) To pass SOILWAT2 output back to rSFSW2; And (3) to create a SQLite weather database for simulation runs. In the the future item (3) will be separated into its own R package. Currently, the STEPWAT2 model is accessing the rSOILWAT2 package for its weather database functionality.

Specifics about downloading, installing, and using rSOILWAT2 can be found in the [rSOILWAT2 README](https://github.com/DrylandEcology/rSOILWAT2/blob/master/README.md).

### rSFSW2 <a name="rsfsw">

The rSFSW2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSFSW2 package. rSFSW2 is a wrapper for the SOILWAT2 model. The primary functions of the rSFSW2 packages are to: (1) Handle input data from multiple sites efficiently; (2) Grab additional inputs from stored or online resources; (3) Calculate additional inputs based on user treatment and experimental design options; And (4) Aggregate output from SOILWAT2.

Specifics about downloading, installing, and using rSFSW2 can be found in the [rSFSW2 README](https://github.com/DrylandEcology/rSFSW2/blob/master/README.md).

### STEPWAT2 <a name="stepwat">

The STEPWAT2 repository contains the __C++__ code, as well as the testing and documentation, pertaining to the STEPWAT2 model.

Specifics about downloading, installing, and using STEPWAT2 can be found in the [STEPWAT2 README](https://github.com/DrylandEcology/STEPWAT2/blob/master/README.md).

### rSFSTEP2 <a name="rsfstep">

The rSFSTEP2 repository contains the __R__ code, as well as the testing and documentation, pertaining to the rSFSTEP2 model.
It interfaces with the STEPWAT2 C code and runs in parallel for multiple sites, climate scenarios, disturbance regimes, and time periods.

Specifics about downloading, installing, and using rSFSTEP2 can be found in the [rSFSTEP2 README](https://github.com/DrylandEcology/rSFSTEP2/blob/master/README.md).

### rSFSW2_tools <a name="sfsw_tools">

The rSFSW2 tools repository contains test projects that assists in examples of use and formal and informal testing for the rSFSW2 package. These test projects rely on the master branch of rSFSW2.

Specifics about running or updating the projects in rSFSW2_tools can be found in the [rSFSW2_tools README](https://github.com/DrylandEcology/rSFSW2_tools/blob/master/README.md).

### Additional Repositories <a name="addrepos">

There are additional "legacy" repositories with old code and information, that are not currently active. These include *STEPWAT2.testingfolders*, *SoilWatExplorer*, *JsoilWat*, *JStepWat*, and *stepwat_spinup_test*.

## Relationships between Repositories <a name="repoRelations"/>

SOILWAT2 runs on a site by site basis. While the simulations of water balance are fast, running SOILWAT2 stand-alone is inefficient, as it needs to read and write information from disk about each site individually. Furthermore, running simulations for many sites is cumbersome from the user perspective, as a series of site-specific input files (.in) need to be formatted for each site. Because of these issues, the R Soilwat Wrapper (rSFSW2) and the R Soilwat Package (rSOILWAT2) were developed. These repositories allow users to set-up information for multiple sites simultaneously and for the efficient handling of these sites' information in R.

rSOILWAT2 is an R package with a series of functions that passes information as a S4 class object between R and C. rSOILWAT2 knows nothing about your site or water-balance; It is simply a package with functions designed to interface between the two programs. rSOILWAT2 avoids disk operations and most things happen in memory. Information, read-in, or calculated, and formatted by rSFSW2, is passed via rSOILWAT2 to SOILWAT2. Most importantly, it allows for the execution of SOILWAT2 to happen completely in memory.

rSFSW2 is designed to gather user's options and information about sites. All site-specific inputs and treatments in a project are read in at once via a series of _.csvs_. The information within these .csvs is then parsed up on a site by site basis, processed, and then sent to SOILWAT2 via the functions in rSOILWAT2. Outputs from the SOILWAT2 model are then sent back to rSFSW2 via rSOILWAT2 and either saved on a site by site basis, or aggregating and stored in a SQLite outputs databased.

## Git and GitHub  <a name="gits"/>

As you may have guessed by being here, Git and GitHub are at the heart of our workflow. GitHub, and alternatives such as BitBucket and GitLab, are web-based hosting services for the use of Git. Git is a free and open source version control system (VCS) that is responsible for all version control programming that happens locally on your computer. Git may need to be installed on your computer. Downloads available here: https://git-scm.com/downloads.

Git and GitHub are important to our group because it provides a way to track changes to our code, and to communicate code purpose, as well as our plans and concerns to one another. While Git provides the platform for communication, there are still stringent rules we abide by to ensure clarity and control.

### Interacting with Git <a name="interacting"/>

The __terminal__ is the defacto interface with Git and all Git commands start with 'git'. Typing 'git' into terminal will yield a list of common commands. Much of the functionality for git command line is replicated in __Git GUIs__ or is available online at GitHub.com.

Git GUIs are best for pushing commits and _tracking changes and the history that other users have made_, but lack some advanced features (e.g., management of sub-modules). Tracking file by file and line by line changes allows for the developer to avoid careless mistakes (i.e. did you really want to add that blank line there?), to double check their work and changes across files, and to be aware of changes made by other developers. The Dryland Ecology organizations prefers a combination of ['GitHub Desktop'](https://desktop.github.com/) for Mac OSX and Microsoft Windows (and ['GitKraken'](https://www.gitkraken.com/download) for Linux machines) _and_ [Atom](https://atom.io/). Atom is a git embedded text editor. From atom, you can switch branches, stage and commit changes, and resolve merge conflicts.

In regards to code development, GitHub.com is best for submitting pull requests and easy merging when there is _no_ conflicts. We also use GitHub for its organizational and communication capacities (discussed below).

### Developing with Git <a name ="developing"/>

We use the ['Github flow'](https://guides.github.com/introduction/flow/) (a 'feature branch workflow') as the basis for our projects. Code is never developed on the master to keep the master branch clean and functional within our tested expectations.

Our essential workflow has 11 steps: (1) Set user configurations (2) Clone repository to a local folder -> (3) Create Issue -> (4) Create a feature branch to work on this issue -> (5) Develop code locally -> (6) Commit and push changes with __useful__ commit messages from the local to the global -> (7) Merge master into feature branch -> (8) Open a pull request -> (9) Ensure that branch passes continuous integration tests -> (10) Merge development branch into master & close PR -> (11) Update version on master.

More information about each of these steps can be found [here](git_workflow.md).

### GitHub Organization Features <a name="gitcomm"/>

Follow our code of conduct in all communications, e.g., [Contributor Code of Conduct of the rSFSW2 repository](https://github.com/Burke-Lauenroth-Lab/rSFSW2/blob/master/CONDUCT.md).

* We use __issues__ and __milestones__ to communicate code enhancements, bugs, priorities, and current progress.

* We have __three types of branches__:
    * __Master branch__: Any commit on the master branch aims to be deployable. Such commits are usually the result of merging/rebasing with a feature or bugfix branch. Once deployable a unique version number is released. Deployable for us means that commits are tested.
    * __Bugfix branches__: When a bug is discovered on the master branch, a bugfix branch and an issue should be created. The issue should be assigned to the _master_ milestone. A bugfix branch should be named after the respective issue. An example of a bugfix branch would be bugfix_16, which represents issue #16.
    * __Feature branches__: Everything else should be a feature branch, which is where code development is done before it is merged back to master. Each feature branch needs its own __milestone__. Any bugfixes needed on a feature branch should be directly committed to the feature branch. Feature branches should have descriptive but concise names in upper camel case, such as feature_BetterErrorMessages.

#### Milestones <a name="milestone"/>
Milestones map to branches within a repository, and hold issues relating to that branch. Whenever you create a _feature_ branch, you should also create a respective milestone.
* Name it the exact same as the branch name, minus the "feature_" prefix
    * For instance, _feature_CO2Effects_ should be named _CO2Effects_
* Provide an apt description of the branch
* Optionally, provide a deadline

#### Projects - Repository Level <a name="repoprojects"/>
Projects on GitHub are meant to assist in the organization and prioritization of the implementation of broad and/or future ideas. Each project consists of  three different columns: To Do, In Progress, and Done. In each of these columns, the user can broach different tasks and ideas that are free-from or that directly reference existing issues.

#### Projects - Organization Level <a name="organizationprojects"/>
Projects at the organizational level are similar to those at the repository level, except that they can span multiple repositories. This is useful for the implementation of features that have, for example, both a C (e.g. SOILWAT2) and R (e.g. rSOILWAT2) component.

## Git Code Quality Features <a name="gitqual"/>
* CI
* codecov
* versioning


## Hierarchy of Testing <a name="testheir"/>
We execute a series of tests at different levels to detect bugs and to validate and verify that our code is acting as expected.
We test at a series of different _levels_. From lowest to highest, these are:

* __Unit Tests:__ Unit tests test the smallest testable part of a software. We write unit tests around different __functions__, checking that the output is as expected, based on function inputs and arguments. These tests are completely user-generated, and force us to design and write good code. Unit tests also allow for the easy and pointed detection of an error in the code, as we make changes and add functionality.

* __R Package Tests:__ As the name suggests, these tests are for R packages only (i.e. rSFSW2 and rSOILWAT2). R package tests execute all unit tests for that package, as well as a series of other checks aimed at detecting common problems. [More information here.](http://r-pkgs.had.co.nz/check.html)

* __Continuous Integration & GitHub Checks:__ GitHub checks are automated so that feature or bugfix branches cannot be merged / a pull request cannot be approved until these checks are passed. Continous integration (CI) is the automation of building, testing, and validating code as new commits are made, across platforms. CI, as the name implies, occurs continually so that the source of error or conflict are more easily tracked. The master branch is always kept clean, and CI checks are tested as pull requests are submitted. Our CI GitHub checks currently consist of coverage checks and platform build checks.
  * Coverage checks look to see that the percentage of line of code covered by unit tests has _increased_. If it hasn't the test will not pass.
  * Platform build checks are tested on a Unix and Windows Servers using Travis and Appveyor, respectively. Travis and Appveyor are servers with these OSs where the program is built and checked. This is useful because if, for example, SOILWAT2 is not building on an individual's Linux computer, but is building on Travis, we know the user's computer is not configured correctly, as opposed to a software design issue.

* __Test Projects:__ Testing at the comprehensive level to test that new code or feature is still producing the same or sensible output. Additionally, test projects provide an example of how the user should set up a project for it to function properly. Checks are made on speed and against reference databases.

## R Code Standards <a name="rinfo"/>

  ### Style Guide <a name="rstyle"/>
  * __Style guide__: In development. For R coding style, DRS suggests to follow [Hadley Wickham's style guide for R](http://adv-r.had.co.nz/Style.html). There are many other R style guides including: [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml), [Bioconductor's Coding Style](https://www.bioconductor.org/developers/how-to/coding-style/), [rDatSci/rOpenSci's R Style Guide](https://github.com/rdatsci/PackagesInfo/wiki/R-Style-Guide), [R Coding Conventions by H. Bengtsson](https://docs.google.com/document/d/1esDVxyWvH8AsX-VJa-8oqWaHLs4stGlIbk8kLc5VlII/edit), [4D R code style guide](https://4dpiecharts.com/r-code-style-guide/)

  ### Code Development <a name="rdevel"/>

  ### Documentation <a name="rdoc"/>

  ### Unit Testing <a name="rtest"/>

  We use the 'testthat' framework for unit testing in R. Within each of our R package repositories there is a _tests/testthat_ directory. Within the _tests_ folder there is testthat.R, which guides R to to test all files in the _testthat_ folder during the R CMD check. Each file within the _/testthat_ folder should begin with __test__ and each of these files contains multiple related tests. We typically have _at least_ a test file for each corresponding file in the /R folder, but it is possible that there might need to be many test files for each /R file.

  Each test should be designed to test the expectation or multiple expectations of a function. Is the output the right value or the right class? Are the values equal to a reference value? Is there an error message, warning, or message when we want there to be?  There is more information on the variety of available test functions and how to use theme [here.](http://r-pkgs.had.co.nz/tests.html#test-tests)

  Each and every function should have a test. Our goal is 100% code coverage. As you write tests, check that they are working by with Ctrl/Cmd-Shift-T or devtools::tests().

  #### An Example


## C Code Standards <a name="cinfo"/>

  ### Style Guide <a name="style"/>

  ### Code Development <a name="cdevel"/>

  ### Documentation  <a name="cdoc"/>

  We use Doxygen to write documentation in C. The style guide and rules for Doxygen can be found [here](http://www.stack.nl/~dimitri/doxygen/)

  To create a document from your Doxygen code, navigate to the desired directory (i.e. 'Git/SOILWAT2'), and then simply execute the 'Doxygen' command in the terminal. This should create a .html file in /doc folder that you can open in any web browser.

  Refer to our C documentation checklist each time you are plan to commit changes with alterations to C documentation.

  ### Unit Testing <a name="ctest"/>

  #### Formatting

  * Be consistent with indentation!! Typically you indent in once every time a curly bracket is opened and out when the bracket is closed. Program like atom will do this automatically for you.
  * Notation, notation, notation!
  * On the line preceding each main test (TEST{}), please write a not describing what function this test is testing.
  * Within the TEST({}) brackets the format for naming the  test is (NameofDoc, NameOfFunction). For example, (SWFlowTest, potSoilEvapBS)..
  * Inputs should be declared first in the TEST. You should try and minimize the number of lines input declaration consumes. For example, you can declare all double that are a length of 1 on one line.
  * There should be only one definition of a variable before each unit test. Avoid declaring it in the inputs and then defining it again before a unit test, without the variable ever being called.
  * Break your TEST code into blocks based on assertions (EXPECT_). Write text (i.e. “Testing when biolive is greater than shade”) and then put everything (defining or re-declaring inputs, running the function, assertions) below this text. For example:
  ```
       // Inputs
       double biolive, shade;

       // Test when biolive is greater than shade
       biolive = 400;
       shade = 300;

       bioliveFunc(biolive, shade);
       EXPECT_GT(0, biolive);//We expect the results to be X when biolive is X
   ```
  * Use a sensible and consistent amount of line spaces: i.e. one space between inputs and functions, etc.

  #### Conditions and assertions

  * For functions that calculate related vales across layers (nlyrs) or simulation (nRgr) you should always test when these each each one (i.e. nlyr = 1) and under the maximum conditions (n_lyr = MAX_N_LYR) if possible. All unit tests should be run twice under each of these conditions.
  * An if or if else statement should typically be tested, if possible.
  * Never calculate an expected answer within the TEST and test an output against it.



## Text Editors  

  There are two types of text editors: (1) Those accessed via the command line and (2) editors with GUIs.

  __Command line editors__ are typically used for quick edits in the terminal. Options include nano, vim, and vi. Nano has the most user-friendly interface. Most all shortcuts are listed at the bottom of the window, there is autotomatic indentation, and many search functions.

   * `git config --global core.editor <editor>`

  __GUI text editors__ are typically more user-friendly, particuarly when approaching projects with multiple interacting repositories. Multiple "projects" (i.e. repositories) can be opened and stored. Atom and Sublime text are two of the most popular. Atom is developed by GitHub and tracks the git status of files (i.e. highlights files in the file tree if they have been changed, shows which lines have been added, deleted, or editted on the left pane of the file folders, basic merge conflict resolution).

  __Integrated Development Environments__ or __IDEs__ are software suites that contain multiple tools needed to test and write software. Typically they not only include a GUI text editors, but also a compiler and debugger to test and clean code. IDEs are language dependent (i.e. you need an IDE that can compile C code for SOILWAT2 and one for R code to compile rSFSW2). RStudio can act an an IDE for R code, while NetBeans is a popular choice for C development.


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
