# Git

## What is Git

`Git` is a version control system (`VCS`) that allows storing and tracking changes of a file or a set of files over time, in a `repository`. 

 The big advantage of this system is that as the files keep getting updated and `committed`, `Git` will keep track of all of its versions since the very first one, allowing the `owner` to revert back to previous states if needed. 

Another major feature of this system is the ability for the `repo` to receive contributions (that must be approved by the owner/admin of the `repository`) from other users, also keeping track of who contributed with what and when. 


## Repositories

Term used to represent a named project where all of the added and tracked files exist. All of the repository history and state data are contained inside the `.git` directory, which is created locally at the root of the project and updated as more changes/versions are committed. 

## Status

When a repository is created, we then start working on files inside the directory, editing and saving them as we would normally do. But in `Git`, at this point in the repository, the files are `untracked`. The status of the files can be checked with the `git status` command. This will show if any files were modified and need to be `staged`, or, if they are already `staged` and need to be `committed` to `Git`. 


## Staging 

When files are modified, even though they live inside the local directory, they are not added to the actual `staging area` (`index`) yet. To do this, we need to `stage` them so they can become `tracked`, using the command: 

```bash
git add <filename> 
```
or, to stage all of the files with changes:

```bash
git add . 
```

## Commit 

After `stage` we will still need to add the current state of the tracked files to `git`, so that a new version is added to the repository's history. This action is called `commit` and its command is executed adding a concise text describing what changes are getting committed:

`git commit -m "some changes in some file/files"`

## Log

Git command that outputs the current history of commits of the repository.


## Porcelain and Plumbing Commands

High-level commands are referred to as `porcelain`. These are the `Git` commands that will be used during the majority of the time, such as the ones I have described previously. 

`Plumbing` is term used to represent the low-level `Git` commands.
An example of a `plumbing` command is: 

`git cat-file -p <hash>`

This outputs a structured representation of the commit object belonging to the passed `<hash>` argument.  

## Git Config

Command that allows the user to list/add/update/delete data from the Git configuration.

## Branch

When working on a project it is frequently needed to experiment different possibilities with the code files. By having the ability to use the Git `branch` feature, we can separate a path from the main history. After branching out from the main path, we can execute changes and proceed with staging and committing without affecting the main branch. 
This is a practical and safe method to experiment with changes since we can simply go back to the base branching point of the main path (`merge base`), or, if we consider that the alternate branch (`feature branch`) is good to be implemented, `merge` it into the main `branch`. 


## Merge 

After branching off from the main path, there will be 2 lines of committing history. From the `merge base` (common ancestor, branching point) onward, we might have additional commits with file modifications on both branches. At some point, they will need to be merged back together in a `merge commit` point (where a branched path merges back into the main line). 
When merging both branches together in this commitment, we will have all of the modifications from the main branch as well as all of the changes from the `feature` branch. This merging of changes will occur nicely only if none of the changes from both branches collide. After this successful execution, the `merge commit` will have 2 parents, one from the last commit of the feature branch line and one from the last main path commit. 

If any clash of data happens, the `merge commit` won't occur due to a `merge conflict`. This can only be solved and sorted out manually through human intervention. 

## Fast Forward Merge 

When we have a main branch which last commit is the actual `merge base`, and the  `feature` branch with `n` commits needs to be merged back into `main`, a `fast forward merge` occurs instead of a `merge commit`. This is due to the fact that the main branch did not commit any additional changes from the `merge base` forward, meaning that the `feature` branch contains all of the history of 'main'. Because of this, the pointer at the tip of the feature branch will be the new commit `head` in the main path, since no alternate branches are needed. 

## Rebase

Rebasing is possible under conditions where we have a main branch and a feature branch, both with addtional commits. Rebase allows us to move the `merge base` where both lines diverged forward until the `head` of the main branch. In practice, this means that the branching point will be placed at the `head` of main, establishing conditions for a `fast forward merge`, if needed, since the new `merge base` will not have any additional commits on the main `branch`. 


## Reset

### Git Reset Soft 

Soft reset lets us go back to a previous commit and undoes changes without deleting them, keeping these `staged` (modified files remain in disk). 

### Git Reset Hard 

Also goes back to a previous commit, undoes changes but also deletes all of the current `staged` changes (removes the modified files).

## Remote 

Remotes are repository versions of our local repo. These can be set up locally as well but, they are frequently used as `remote` versions of our local code, in a central repository, preferably a web hosting platform (`Github` for example). They are set up as the `origin` repo, such that we, as developers, can `push` and  `pull` updates at any given time, from any machine. 

## GitHub

Web hosting platform that has the purpose of serving as a central location to store Git `remote` repositories. Used to store and backup projects and code, as well as making them available to other users that wish to `clone`or `fork` them, allowing the possibility to contribute with changes.   

### Push 

Command used to `push` the current repository state into the `remote`, which in this case, would be the `origin` central repository hosted in `GitHub`.

### Pull 

`Fetch` and `merge` the current state of the repo from the `origin` (GitHub or any other `remote`) to our local repository. This `pull` command does 2 things, `fetch` and `merge`. 

`Fetch` is a Git command that copies all of the state from another repository, without merging/changing any of the current state of our local files. To actually implement the changes, we need to use the `merge` command. 

`Pull` does these 2 things in one go. 


### Pull Requests

Act of requesting for changes to be reviewed and merged into a central repository. For example, if we `fork` a public repo from `GitHub` and decide to work on some changes locally, we can execute a `pull request` with the intent of asking for those changes to be reviewed and implemented/merged.  


## Gitignore

 A hidden file or files that live in different paths levels of the local repository, where we can specify which files or directories should be ignored. This means than none of them will be `tracked` and `committed` to Git. 

 This is needed to avoid tracking large data that is only needed for runtime or compilation purposes, like large assets, executables, dependencies or generated files. 
 
 It is also important to add and ignore configuration files that contain credentials, keys and other sensitive data.  
 












