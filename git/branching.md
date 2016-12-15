To really understand the way Git does branching, we need to take a step back and examine how Git stores its data.

As you may remember from Getting Started, Git doesn’t store data as a series of changesets or differences, but instead as a series of snapshots.

When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the content you staged. This object also contains the author’s name and email, the message that you typed, and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one parent for a normal commit, and multiple parents for a commit that results from a merge of two or more branches.

## Creating a New Branch
What happens if you create a new branch? Well, doing so creates a new pointer for you to move around. Let’s say you create a new branch called testing. You do this with the git branch command:
```
# longqi @ LQMacPro in ~/test_project on git:master o [16:10:29]
$ git branch develop
```

This creates a new pointer with name `develop` to the same commit you’re currently on.
```
# longqi @ LQMacPro in ~/test_project on git:master o [16:10:58]
$ git checkout -b develop

Switched to a new branch 'develop'
```
This creates a new pointer to the same commit you’re currently on and checkout to the new branch.

To see all branchs:
```
# longqi @ LQMacPro in ~/test_project on git:develop o [16:12:18]
$ git branch
* develop
  fix_bug_112
  master
```

## Switching Branches
To switch to an existing branch, you run the git checkout command. Let’s switch to the new testing branch:
```
$ git checkout develop
```
This moves HEAD to point to the develop branch.