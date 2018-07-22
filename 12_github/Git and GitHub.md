# Intro to Version Control with Git and GitHub
This curriculum will cover the concepts related to Version Control and why it's an imoportant part of the development lifecycle

## Useful Resources
[Git Website](https://git-scm.com/)  
[Official Git Online Tutorial](http://try.github.io/)  
[Git Command Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)  
[Sourcetree GUI client](https://www.sourcetreeapp.com/)  
[The Free Git book](https://git-scm.com/book/en/v2)  
[Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)  
[Git Reference](https://git-scm.com/docs) 

---

1. What is Version Control?
    1. Centralized vs Distributed Version Control
2. Git Basics
    1. [Exercise - Your first repository](exercises/first_repository/)
3. Branching and Merging
    1. [Exercise - Create and merge a branch](exercises/branch_and_merge/)
4. Typical Git Workflow
5. GitHub - Sharing your code with the world
    1. Some comming GitHub terminlogy
    2. Common commands when working with remote repositories
    2. [Exercise - Put your repo on GitHub](exercises/repo_on_github/)
6. Homework

---

## What is version control?
Version control systems help development teams keep track of changes to their source code over time. It is also commonly referred to as SCM (Source Code Management). Version Control Systems keep track of every modification made to the code base and allow you to go back and forward in time as it relates to changes in your source code. It also facilitates comparing code at different points in time to help fix any bugs or issues that may have been the result of a specific change.

### Centralized vs Distributed Version Control
For this course we are going to focus on Distributed Version Control with Git but it is worth discussing the difference between Distributed and Centralized Version Control. 

**Centralized Version Control**  
Centralized Version Controls systems like SVN (Subversion) require a single “central” copy of your project typically on a server somewhere. Developers “commit” their changes to this central copy. If this central copy is not available then the developer is not able to save changes to their work within the Version Control System until they have access to the server.

***Typical Workflow for CVC***  

* Developer checks out code from version control or pulls down latest changes to code already checked out.
* Developer makes some changes and tests his work
* Developer commits changes back to central repository for other to pull into their local copies

**Distributed Version Control**  
Distrubuted Version Control sytems like Git do not rely on a central server to store all the versions of a project’s files. You can however utilize a service like GitHub to simulate this functionality. Every developer would “clone” an existing repository from a service like GitHub or initiate a repository locally. A copy of a repository and the full history of the project is stored locally with no need to connect to the centralized server to track changes. The developer can make changes to the source code and commit those changes to the local repository on their system. When they are ready to share changes with other developers they can "push" their changes and all the attached history to the remote repository. Once pushed, other developers can now update their local copies with those changes. 

***Typical Workflow for DVC***  

* Developer checks out code from version control or pulls down latest changes to code already checked out.
* Developer makes some changes and tests his work
* Developer makes a local "commit" that repesents the changes made
* Developer repeats this cycle until locally until they are ready to share their changes with other developers
* Developer commits changes back to central repository for other to "pull" into their local copies or provide a patch file to another developer if they choose not to use a service like GitHub

## Git Basics
This section will cover the basics of working Git on your local machine

**Common Basic Commands**

* `git` - This is the top level command. You will typicially type `git` and then an action you want git to execute. You may remember this command when we checked what version of git we had installed by running `git --version`
* `git init` - This will tell git that the directory you are in when this command is run should be tracked by git. Your are creating the repository when you run this command
* `git add` - followed by a path will let git know you want to add files at that path to be tracked by version control and staged for commit
* `git commit` - Takes your staged files and commits them to version control and creates a point in your version control history that can be referenced later. You will almost always find the `-m "COMMIT MESSAGE"` flag used to provide a message related to your commit.
* `git status` - Displays the files that have changed in your repo as it relates to the latest commit. It will show what files have been modified and which files are staged for your next commit. It also shows information about which branch you are currently working with.
* `git diff` - Displays the line by line differences between files in your repo compared to the last commit

##Branching and Merging
Branching is the concept of going down a different path in the code base from a specific commit. Usually branching off master in it's latest state to make a particular set of changes and not modifying the state of the the original code you branched from. Merging is the concept of applying changes made in one branch to another. 

***Practical Example***  
You need to make some changes to a project that may take some time. You make a branch to do those changes. An emergency change comes in that has to be pushed live. Instead of having your changes for your feature in the way, you can switch back to the master branch, create another branch for those changes, merge the emergency changes into master and push it live. Update your feature branch with whats now in master using a merge and continue your work. This avoids your changes that may take some time to complete from being in the way.

**Merging and Branchin Commands**

* `git branch` - Shows you what branches exist in your repository and what branch you are on. Your current branch is marked with a `*`
* `git checkout -b BRANCH_NAME` or `git branch BRANCH_NAME` - git checkout -b creates a branch and checks out that branch. git branch with a branch name provided creates the branch but does not check it out.
* `git branch -d BRANCH_NAME` - Deletes a branch.
* `git merge BRANCH_NAME` - Merges the named branch into the branch you are currently on. All the changes that exist in the branch you are merging from now exist in your branch.

## Typical Git Workflow
This section will cover a basic git workflow for making changes to a repository tracked with Git.

![Git Workflow](resources/images/git_workflow.jpg)

***It is best practices to create a branch for each logical grouping of changes you are making to your project.***

***Practical Example***  
You have been assigned a task to add a header to the website. You would create a branch for this task called something like "AddingHeaderToWebsite". You would create your branch of your up-to-date master branch. Once the branch is created you would start working on adding the header to the webstie. You should make a commit after each meanigful milestone in your development process. Once you are done with the header and tested everything on your branch, you would make sure your master branch is up-to-date incase anyone changes have been made to master since creating your branch. You would then merge master into your branch and handle any conflicts. Once confilicts have been resolved, if any came up, you would then merge your "AddHeaderToWebsite" branch into the master branch. Since you no longer have a need for your "AddHeaderToWebstieBranch" it is safe to delete it. All the history and commits from your branch have been merged into master so you can still go back to all the commits you made in your branch.

##GitHub - Sharing your code with the world
GitHub is a service that allow you to make your Git repositories available to other developers and the world through a Web-based graphical interface. It has opinions on how to grant access to your code as well as how people can contribute to your code. It also provides several collaboration features, such as a wikis and basic task management tools for every project.

### Common GitHub terminology

**Repository**  
A repository is a location where all the files for a particular project are stored, usually abbreviated to “repo.” Each project will have its own repo, and can be accessed by a unique URL.

**Forking**  
“Forking” is when you create a new project based off of another project that already exists. If you find a project on GitHub that you’d like to contribute to, you can fork the repo, make the changes you’d like, and release the revised project as a new repo. If the original repository that you forked to create your new project gets updated, you can easily add those updates to your current fork. You can also request that changes you have made can be merged into the original project. This is facilitated through Pull Requests.

**Pull requests**  
A pull request is a way of facilitating a merge through the GitHub interface. If you or someone on your development team has made some changes in a branch or a forked repository, you can create a pull request asking for those changes to be merged into the master branch or any other branch that exists in the repo. The owner of the project can then review your changes and merge those changes if all is good. Pull requests can be created from forked repos as well.

### Common commands when working with remote repositories

**`git remote add`**  
To communicate with the outside world, git uses what are called remotes. These are repositories other than the one on your local disk which you can push your changes into (so that other people can see them) or pull from (so that you can get others changes). The command `git remote add NAME_OF_REMOTE URL_OF_REMOTE` creates a new remote called whatever you put in place of NAME_OF_REMOTE located at URL_OF_REMOTE. NAME_OF_REMOTE is usally origin and URL_OF_REMOTE is the link to your GitHub repository. This lets git know that you are connected to a remote repository.

**`git push`**  
This is a command that says "push the commits in the local branch to the remote branch". Once this is executed, your commits since your last push will be available on GitHub. Your initial push may looks someghing like `git push -u origin master` which says push my code to my remote named `origin` into the master branch. Once this has been done the first time you can just use `git push` from that branch and dont need the rest of the command.

**`git pull`**  
This is a command that says "pull the commits in the remote branch to the local branch". Once this is executed, any commits made to the remote branch since your last pull will be available in your local copy of the repository. 

**`git clone`**  
`git clone` is usually followed by the url of the remote repository on GitHub you would like to "clone" or make a local copy of on your computer. On any repo you will see a "Clone or download" button. Clicking that button will provide you with the url required to clone that repo.   

## Homework
Go through the GitHub Training available at https://try.github.io/
