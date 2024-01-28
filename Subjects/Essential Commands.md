# Essential Commands 20%:
* [Basic Git Operations](https://github.com/DorBitton/LFCS/blob/main/Subjects/Essential%20Commands.md#basic-git-operations)
* Create, configure, and troubleshoot services
* Monitor and troubleshoot system performance and services
* Determine application and service specific constraints
* Troubleshoot diskspace issues
* Work with SSL certificates


## Basic Git Operations

### Installation
* Install Git on your system using the following command:

    * `sudo apt install git`

### Configuration
* Set your username
    * `git config --global user.name "YourUsername"`

* Set your email
    * `git config --global user.email "youremail@example.com"`

This is not credentials, it's just to inform who is commiting actions.

### Getting Started
* Initialize a new Git repository in a folder:
    * `git init`

### Staging and Commiting Changes: 
1) Making changes in the working area.
2) Adding the changes we want to track with Git to the staging area.
    * Check the status of changed files:
        * `git status`
    * Add changes to the staging area:
        * `git add .` will add all changed files to the staging area
        * `git add "*.html` will add all html changed files to the staging area
    * Unstage a specific file if needed:
        * `git reset filename`

3) Commiting our changes.
    * Commit your changes with a descriptive message:
        * `git commit -m "Add a concise commit message here"`


### Git Branches
* The Master branch is usually the stable version we are currently working with, we will usually merge different branches into the Master. Master is the default branch.
     * `git branch "name"` Create a new branch.
     * `git branch --delete "name"` Delete a branch.
     * `git branch --list` Shows all branches.
     * `git checkout "branch name"` Switch to a branch.

### Viewing Changes
* View commit history:
    * `git log`
    * `git log --raw` Shows all changes with more details. A = added, D = deleted, M = modified.
    * `git show "hashnumber"` Show what changed in a specific commit.

### Merging Changes
* Merge a branch into the working branch:
    * `git merge "branch name"`

### Remote Repositories
* Show connected repositories:
    * `git remote -v`
* Set up a remote repository connection:
    * `git remote add origin "connection string"` "Origin" is an alias to the connection string.
* Configure SSH keys for login:
    * `ssh-keygen` saved in ~/.ssh.id_rsa.pub and in github page we can add the key in the settings tab. 

### Pushing and Pulling
* Push changes to the remote repository:
    * `git push origin master`, "Origin" Alias to our repository. "Master" the branch to push into.
* Pull changes from the remote repository:
    * `git pull origin master`.

### Cloning a Repository
* Clone a repository using its connection string:
    * `git clone "connection string"`
