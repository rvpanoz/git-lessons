# git --stupid-content-tracker

Git - Distributed Version Control System (DVCS)

## Getting started

This lesson will be about getting started with Git. Explaining some background on version control tools, then move on to how Git works and start working with it.

---

- [About version control](#about-version-control)
  - [Local Version Controls Systems](#local-version-control-systems)
  - [Centralized Version Control Systems](#centralized-version-control-systems)
  - [Distributed Version Control Systems](#distributed-version-control-systems)
- [What is GIT and how it works](#what-is-git-and-how-it-works)
  - [Snapshots, Not Differences](#snapshots-not-differences)
  - [Nearly Every Operation Is Local](#nearly-every-operation-is-local)
  - [Git Has Integrity](#git-has-integrity)
  - [Git Generally Only Adds Data](#git-generally-only-adds-data)
- [The Three Stages](#the-three-stages)
- [Initializing a Repository in a directory](#initializing-a-repository-in-a-directory)
- [Cloning an Existing Repository](#cloning-an-existing-repository)
- [Recording Changes to the Repository](#recording-changes-to-the-repository)
- [Git Branching](#git-branching)
  - [Keep your branch strategy simple](#keep-your-branch-strategy-simple)
  - [Use feature branches for your work](#use-feature-branches-for-your-work)
- [Git exercises](#git-exercises)
  - [Setting up the repository](#setting-up-the-repository)
    - [git clone](#git-clone)
    - [git config](#git-config)
  - [Using branches](#using-branches)
    - [git checkout](#git-checkout)
    - [git branch](#git-branch)
  - [Saving changes](#saving-changes)
    - [git add](#git-add)
    - [git status](#git-status)
    - [git commit](#git-commit)
    - [git stash](#git-stash)
  - [Undoing changes](#undoing-changes)
    - [git reset](#git-reset)
    - [git clean](#git-clean)
  - [Collaborating](#collaborating)
    - [git push](#git-push)
    - [git pull](#git-pull)
    - [git fetch](#git-fetch)
    - [git merge](#git-merge)
  - [.gitignore](#.gitignore)

---

## About Version Control

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
Using a VCS also generally means that if you screw things up or lose files, you can easily recover.

### Local Version Control Systems

- copy files into another directory - sometimes with a timestamp :)
- simple database that kept all the changes to files under revision control
  ([RCS](https://www.gnu.org/software/rcs/))

:+1: Easy setup

:-1: High error proned

### Centralized Version Control Systems

- Collaboration with others
- Single server that contains all the versioned files, and a number of clients that check out files from that central place. (CVS, Subversion)

<p align="center">
  <img alt="centralized" src="./assets/images/centralized.png">
</p>

:+1: Everyone knows to a certain degree what everyone else on the project is doing.
:+1: Far easier to administer a CVCS than it is to deal with local databases on every client.

:-1: Single point of failure that the centralized server represents.

### Distributed Version Control Systems

- Clients don’t just check out the latest snapshot of the files; rather, they fully mirror the repository, including its full history.

<p align="center">
  <img alt="distributed" src="./assets/images/distributed.png">
</p>

:+1: If any server dies, and these systems were collaborating via that server, any of the client repositories can be copied back up to the server to restore it
:+1: Every clone is really a full backup of all the data

:-1: Slow initialization

## What is GIT and how it works?

GIT is Distributed Version Control System. DVCS for short. The major difference between Git and any other VCS (Subversion and friends included) is the way Git thinks about its data.

### Snapshots Not Differences

Other systems (CVS, Subversion, and so on) think of the information they store as a set of files and the changes made to each file over time (this is commonly described as `delta-based version control`).

<p align="center">
  <img alt="deltas" src="./assets/images/deltas.png">
</p>

> Git thinks of its data more like a series of snapshots of a miniature filesystem.

With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored.

`Git thinks about its data more like a stream of snapshots.`

<p align="center">
  <img alt="snapshots" src="./assets/images/snapshots.png">
</p>

### Nearly Every Operation Is Local

Most operations in Git need only local files and resources to operate — generally no information is needed from another computer on your network.

- You have the entire history of the project right there on your local disk. This means you see the project history almost instantly.

- If you get on an airplane or a train and want to do a little work, you can commit happily (to your local copy) until you get to a network connection to upload

### Git Has Integrity

Everything in Git is checksummed before it is stored and is then referred to by that checksum. This means it’s impossible to change the contents of any file or directory without Git knowing about it.

### Git Generally Only Adds Data

When you do actions in Git, nearly all of them only add data to the Git database. After you commit a snapshot into Git, it is very difficult to lose, especially if you regularly push your database to another repository.

## The Three stages

Git has three main states that your files can reside in: modified, staged, and committed:

- `Modified` means that you have changed the file but have not committed it to your database yet.

- `Staged` means that you have marked a modified file in its current version to go into your next commit snapshot.

- `Committed` means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project: the working tree, the staging area, and the Git directory.

<p align="center">
  <img alt="areas" src="./assets/images/areas.png">
</p>

The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.

The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.

The basic Git workflow goes something like this:

1. You modify files in your working tree.

2. You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.

3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

## Initializing a Repository in a directory

First we have to check if git is installed in our machine running the command in the terminal:

```
$ git --version
```

Then navigate to the project's directory for example

```
$ cd /home/user/my_project (linux)
$ cd c:/Users/user/my_project (windows)
$ cd /Users/user/my_project (macOS)

```

and then run the command

```
$ git init
```

This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.

## Cloning an Existing Repository

You clone a repository with git clone <url>. For example, if you want to clone the the repo we are working, you can do so like this:

```
$ git clone https://github.com/rvpanoz/git-lessons
```

If you want to clone the repository into a directory named something other than git-lessons, you can specify the new directory name as an additional argument:

```
$ git clone git clone https://github.com/rvpanoz/git-lessons my-github-lessons
```

## Recording Changes to the Repository

Typically, you’ll want to start making changes and committing snapshots of those changes into your repository each time the project reaches a state you want to record.

> Remember that each file in your working directory can be in one of two states: tracked or untracked.

`Tracked` files are files that were in the last snapshot; they can be unmodified, modified, or staged.

> In short, tracked files are files that Git knows about.

`Untracked` files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area.

> When you first clone a repository, all of your files will be tracked and unmodified because Git just checked them out and you haven’t edited anything.

## Git Branching

Branching means you diverge from the main line of development and continue to do work without messing with that main line. Git branches are effectively a pointer to a snapshot of your changes.

A Git branch is essentially an independent line of development. You can take advantage of branching when working on new features or bug fixes because it isolates your work from that of other team members.

> The “master” branch in Git is not a special branch. It is exactly like any other branch. The only reason nearly every repository has one is that the git init command creates it by default and most people don’t bother to change it.

### Keep your branch strategy simple

Build your strategy from these three concepts:

- Use feature branches for all new features and bug fixes.
- Merge feature branches into the master branch using pull requests.
- Keep a high quality, up-to-date master branch.

### Use feature branches for your work

Develop your features and fix bugs in feature branches based off your master branch. These branches are also known as topic branches. Feature branches isolate work in progress from the completed work in the master branch.

<p align="center">
  <img alt="featurebranching1" src="./assets/images/featurebranching1.svg">
</p>

## Git Exercises

### Setting up the repository

Clone the repository using the following command in terminal

```
git clone https://github.com/rvpanoz/git-lessons.git
cd git-lessons
```

Configure git using the following command in the terminal

```
git config --global user.email "your_email@example.com"
```

### Using branches

Use the git checkout command and switch to the develop branch

```
git checkout develop
```

Create a new branch from the develop branch and switch to it using
the git checkout command

```
git branch feature/my-awesome-feat
git checkout feature/my-awesome-feat
```

or you can use the git checkout command

```
git checkout -b feature/my-awesome-feat
```

### Saving changes

Create a new file in the root directory and named it `Bob.txt`.
Inside that file write `Hello Bob!`

Now, use the git status command to inspect the repository.

```
git status
```

Next use the git add command to add the new file in the staging area.

```
git add Bob.txt // to add only Bob.txt
```

```
git add . // to add all the modified files
```

Now, use again the git status command to inspect the repository.

```
git status
```

Next use the git commit command to add it to your next commit snapshot

```
git commit -m "dev: add Bob.txt file"
```

In case you dont want to commit but save your changes for later use
you can use the git stash command and git stash --apply to restore the changes

```
git stash
```

Finally use the git push command to upload the local repository content to the remote repository.

```
git push
```

> Tip! In case your branch does not exists at the origin you have to use

```
 git push --set-upstream origin my_awesome_branch_name
```

> Tip! in case you want to show a complete log use the git log --one-line to command

### Undoing changes

Open `Bob.txt` file and add some content.

Next use the git add command to add `Bob.txt` change in the working directory to the staging area

```
git add Bob.txt
```

Now using the git reset command you can undo that action

```
git reset
```

And if you want to clear the untracked file use the git clean command with

```
git clean -f -d // -f alias of -force, -d is useful when you want to clear an entire directory
```

Finally use the git reset --hard command to move the HEAD into a specific commit

```
git reset --hard HEAD`1 // remove the last commit
```
