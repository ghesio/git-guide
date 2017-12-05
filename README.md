# GIT guide

## Installing

Installing on OSX - [link](https://git-scm.com/download/mac)

Installing on Win - [link](https://git-for-windows.github.io/)

Installing on Linux - [link](http://book.git-scm.com/2_installing_git.html)

## Introduction

A GIT repository is simply a place where the history of your work is stored. It often lives in a *.git* subdirectory of your working copy - a copy of the most recent state of the files you're working on.

To fork a project (take the source from someone's repository at certain point in time, and apply your own diverging changes to it), you would clone the remote repository to create a copy of it, then do your own work in your local repository and commit changes.

Within a repository you have branches, which are effectively forks within your own repository. Your branches will have an ancestor commit in your repository, and will diverge from that commit with your changes. You can later merge your branch changes. Branches let you work on multiple disparate features at once.

You can also track individual branches in remote repositories. This allows you to pull in changes from another individual's branches and to merge them into a branch of your own. This may be useful if you and a colleague are working on a new feature together.

## Workflow

your local repository consists of three "trees" maintained by git. the first one is your *Working Directory* which holds the actual files. the second one is the *Index* which acts as a staging area and finally the *HEAD* which points to the last commit you've made.

![trees](trees.png)

## Create a new repository

In a directory execute `git init`

## Checkout of a repository

In git terms a *checkout* is the act of switching between different versions of a target entity.

Create a copy of a local repository `git clone /path/of/repository`

To clone a remote repository `git clone username@host:/path/to/repository`

## Add & commit changes

You can propose changes (add it to the **Index**) using `git add <filename>` (or `git add *` to add all modified files).

This is the first step in the basic git workflow. To actually commit these changes use

 `git commit -m "Commit message"`

Now the file is committed to the **HEAD**, but not in your remote repository yet.

## Pushing changes

Your changes are now in the **HEAD** of your local working copy.

To send those changes to your remote repository, execute

`git push origin <branch>`

where *<branch>* is the branch you are working on and you want to push your changes to.

If you have not cloned an existing repository and want to connect your repository to a remote server, you need to add it with

`git remote add origin <server>`

Now you are able to push your changes to the selected remote server.

## Branching

Branches are used to develop features isolated from each other.

The **master** branch is the "default" branch when you create a repository. Use other branches for development and merge them back to the master branch upon completion.

![branches](branches.png)

Create a new branch named *"feature_x"* and switch to it using

`git checkout -b feature_x`

switch back to master

`git checkout master`

and delete the branch again

`git branch -d feature_x`

a branch is *not available* to others unless you push the branch to your remote repository

`git push origin <branch>`

## Updating ad merging

To update your local repository to the newest commit, execute

`git pull`

in your working directory to fetch and merge remote changes.

To merge another branch into your active branch, use

`git merge <branch>`

in both cases git tries to auto-merge changes.

Unfortunately, this is not always possible and results in conflicts.

You are responsible to merge those conflicts manually by editing the files shown by git.

A conflicted change has this aspect:

    <<<<<<< HEAD:file.txt  
    Hello world      
    =======  
    Goodbye  
    >>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt  

Here you need to choose what revision of the conflic you want to maintain by deleting the unwanted one.

After changing, you need to mark them as merged with

`git add <filename>`

before merging changes, you can also preview them by using

`git diff <source_branch> <target_branch>`

## Tagging

It's recommended to create tags for software releases.  
You can create a new tag named 1.0.0 by executing  

`git tag 1.0.0 1b2e1d63ff`  

the *1b2e1d63ff* stands for the first 10 characters of the commit id you want to reference with your tag. You can use anything you want as tag ID.   
You can get the commit ID by looking at the **git log**

## Git log

In its simplest form, you can study repository history using `git log`


You can add a lot of parameters to make the log look like what you want.  
To see only the commits of a certain author:

`git log --author=bob`

To see a very compressed log where each commit is one line:

`git log --pretty=oneline`

Or maybe you want to see an ASCII art tree of all the branches, decorated with the names of tags and branches:

`git log --graph --oneline --decorate --all`

See only which files have changed:

`git log --name-status`

## Reverting and discarding changes

To discard current changes

`git checkout -- *`

If you have a dirty working tree and wanna save changes (before committing or pulling) you can put them in the *stash* by using

`git stash <optional_filenames>`

and retrieve them later by using

`git stash pop` or `git stash apply`

The difference between *pop* and *apply* is that *apply* does not remove the files from the stash.  
You can have more edits stashed, by using `git stash list` you can see them

    $ git stash list
    stash@{0}: WIP on master: 049d078 added the index file
    stash@{1}: WIP on master: c264051 Revert "added file_size"
    stash@{2}: WIP on master: 21d80a5 added number to log

You can get the edits in commit *21d80a5* by using `git stash apply stash@{2}`

To remove an entry from the stash list use `git stash drop <stash>`

By using `git checkout <commit>` you go back to the state of the code at the moment of *commit*, and you are leaved in **detached HEAD state**.  
This is a read only operation.   
(to switch back to last commit use `git checkout <branch_name>`)   
If you want to commit the changes made from this state you need to create a new branch and merge it.

`git branch my-temporary-work`  
`git checkout <original_branch>`  
`git merge my-temporary-work`

And then you have to take care of all the conflicts that raise and push the changes.  
(you can deleted the temporary branch by using `git branch -d my-temporary-work`)

It's also possible to checkout a single file by using

`git checkout <commit> <file>`

NOTE: this is not a read only operation: *file* will be in the staging area.

Another approach is to use `git revert <commit>`  
The `git revert` command undoes a committed snapshot.  
But, instead of removing the commit from the project history, it figures out how to undo the changes introduced by the commit and appends a new commit with the resulting content.

As long as you can stay away from `git reset --hard <commit>`, because it's a risky operation and affects ALL the users working on a repository.

A friendly remainder: don't push not working code because reverting changes and merging it's always time consuming.

## Credits

- [Git - the simple guide](http://rogerdudler.github.io/git-guide/index.html)
- Stackoverflow
