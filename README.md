# Git command manifesto  

> These are the usefull git commands y'all need to work with. 

## Config

    git config --global user.name " "
    git config --global user.email  " "

## Adds everything to the stagging area

    git add .

## Removes a specific file from the stagging area 

```bash
    git rm --cached file.html
    git reset file.html
    #for all files
    git reset 
```

## Stash  

>Sometimes I am not done yet and I need to pause my work and fix up a bug real quick
  
```bash
#Stash the staging area (Working directory clean)
 git stash -u

#Displaying the list and inspecting stash changes
 git stash list
 git stash show

#Getting back what I've stashed
 git stash apply
```
    

## Commit the stagging area 

    git commit -m "comit comment" 

## Getting logs 

```bash
#Logs with details
git log

#Logs in one line
git log --oneline

#Logs by author (part of the name works as well)
git log --author=""

#Logs by date
git log --before="2011-12-01"
```


## Revert commits 

```bash
#get the logs with their hash comit XXXX
git log --oneline

#revert the commit (this is itself a commit and could be revertable as well)
git revert XXXX

#revert the last commit
git revert HEAD
```


## Creating a new working branch

```bash
# with checking into it
    git checkout -b <branchname>

# without checking in
    git branch <branchname>
```


## Deleting a branch 

```bash
git checkout master

# if there's unmerged changes it throws an error
    git branch -d <branchname>

# force deleting even with unmerged changes (unsafe)
    git branch -D <branchname>
```


> I prefer using rebase workflow
>> installing p4merge is recomended

## config p4merge

    git config --global diff.tool p4merge
    git config --global difftool.p4merge.path /Applications/p4merge.app/Contents/MacOS/p4merge 
    git config --global difftool.prompt false

    git config --global merge.tool p4merge
    git config --global mergetool.p4merge.path /Applications/p4merge.app/Contents/MacOS/p4merge 
    git config --global mergetool.prompt false

# Rebase Workflow

## Sync with remote

    git checkout master
    git pull 

## Update branch

```bash
git checkout <branchname>
git rebase master
#if theres conflict reslove it either on editor or run
git mergetool
#then after conflict resolution
git rebase --continue
git add files.conflict
git commit -m "confilic fix"
```


## Push Changes

    git checkout master
    git rebase <branchname>
    git push


## other git cmd

```bash

#having an alias for a command (git smth)
git config --global alias.smth "log --oneline --graph --decorate --all"
#renaming and staging
git mv example.txt demo.txt
#deleting and staging
git rm demo.txt
#tagging (if not commit is specified it will be current commit HEAD)
git tag -a v1.0 -m "release of 1.0"
#deleting tag
git tag -d v1.0
#resetting to a point in time commit hash XXXX (changing the HEAD location)
#to that commit --hard is for wiping out pending changes
git reset XXXXX
#we need to go back even if it doesn't show our commit in git log
git reflog
```


# Advanced stuff

## Amend

```bash
#we forgot to commit some files in the last commit
git add file.html
git commit --ammend --no-edit
```


## Reword

```bash
git rebase -i HEAD~2
#it will show list of the last 2 commits 
#changing "pick" to "reowrd"
#(ESC + I) to modify then save and close (ESC + : + W + Q)
```

## delete

```bash
git rebase -i HEAD~3
#it will show list of the last 3 commits 
#changing "pick" to "drop"
#(ESC + I) to modify then save and close (ESC + : + W + Q)
```

## reorder

```bash
git rebase -i HEAD~N
#it will show list of the last N commits 
#reorder the lines as we want
#(ESC + I) to modify then save and close (ESC + : + W + Q)
```
    
## squashing/fixup

```bash
git rebase -i HEAD~3
#it will show list of the last 3 commits 
#changing "pick" to "fixup" (squash preserve log messages)
#if we fixup the message will meld to last commit before fixup
#(ESC + I) to modify then save and close (ESC + : + W + Q)
```

## splitting commits

```bash
git rebase -i HEAD~3
#it will show list of the last 3 commits 
#changing "pick" to "edit" (navbar and fix bug)
#(ESC + I) to modify then save and close (ESC + : + W + Q)

#it will break me out of a special rebasing branch untill i finish
#so we unstage the commits
git reset HEAD^
#then add the nadvbar files and comitting them
git add navbar.files
git commit -m "navbar"
git add fixBugs.files
git commit -m "fix bugs"
git rebase --continue
```

## Git-archive (exports) downloads examples

```bash
#From a specific tag
    git archive --format=zip --output project.zip v1.0
#From the last commit
    git archive --format=zip --output project.zip HEAD
```


## Cherry pick a commit from one branch and apply it to another

```bash
#we have 2 branches branch1 and branch2
#branch1 has the commit we want
#we check into branch1
git format-patch branch2 -o patches
```

    https://www.youtube.com/watch?v=QtXj9tt-RUE&list=PLCy7dPypkr2pukWr-6gszy6E_Wf-ZfJrM&index=4



## Git Flow

    git flow init                             # setup project to use git-flow

    git flow feature start <feature_name>     # creates a new feature branch called                                                  <feature_name>

    git flow feature finish <feature_name>    # merge feature back into develop branch

    git flow release start <version>          # merge develop to release