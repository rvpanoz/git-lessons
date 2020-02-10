# git --stupid-content-tracker

Git - Distributed Version Control System (DVCS)

## Getting started

This lesson will be about getting started with Git. Explaining some background on version control tools, then move on to how Git works and start working with it.

## About Version Control

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
Using a VCS also generally means that if you screw things up or lose files, you can easily recover.

### Local Version Control Systems

- copy files into another directory - sometimes with a timestamp :)
- simple database that kept all the changes to files under revision control
  ([RCS](https://www.gnu.org/software/rcs/))

Pros

- Easy setup

Cons

- High error proned

### Centralized Version Control Systems

- collaboration with others
- single server that contains all the versioned files, and a number of clients that check out files from that central place. (CVS, Subversion)

<p align="center">
  <img alt="centralized" src="./assets/images/centralized.png">
</p>

Pros

- Everyone knows to a certain degree what everyone else on the project is doing.
- Far easier to administer a CVCS than it is to deal with local databases on every client.

Cons

- Single point of failure that the centralized server represents.

### Distributed Version Control Systems

- Clients don’t just check out the latest snapshot of the files; rather, they fully mirror the repository, including its full history.

<p align="center">
  <img alt="distributed" src="./assets/images/distributed.png">
</p>

Pros

- If any server dies, and these systems were collaborating via that server, any of the client repositories can be copied back up to the server to restore it
- Every clone is really a full backup of all the data

Cons

- Slow initialization

## What is GIT and how it works?

GIT is Distributed Version Control System. DVCS for short. The major difference between Git and any other VCS (Subversion and friends included) is the way Git thinks about its data.

### Snapshots, Not Differences

Other systems (CVS, Subversion, and so on) think of the information they store as a set of files and the changes made to each file over time (this is commonly described as `delta-based version control`).

<p align="center">
  <img alt="deltas" src="./assets/images/deltas.png">
</p>

Git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored.

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
