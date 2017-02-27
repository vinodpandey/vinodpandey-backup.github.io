---
layout: page
title: Git branching model for development
---
# Git Flow

### Introduction

Git branching model for development. 
Based on recommendation at 
[http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)

![Branching model](./images/git-model@2x.png)

### Initial repository setup (creating develop branch)
```sh
git@github.com:vinodpandey/git-flow.git
cd git-flow    
git branch (will show current branch)  
* master  

# create a develop branch  
git branch develop  

git branch  
* master  
develop 

# push develop branch to remote repository  
git checkout develop  
git push origin develop

Total 0 (delta 0), reused 0 (delta 0)
To github.com:vinodpandey/git-flow.git
 * [new branch]      develop -> develop
 
```

### Repository setup in developer machine
```sh
git@github.com:vinodpandey/git-flow.git
cd git-flow   

# view all remote branches  
git branch -r  
  origin/HEAD -> origin/master  
  origin/develop  
  origin/master  

# create a local develop branch (all feature branches will be created from develop)  
git checkout -b develop origin/develop  

git branch  
* develop  
  master  
```


### Development/bug fixes

Development/bug fix is done in develop branch only. master branch should always contain production ready code. 

1. Feature branch - feature for upcoming/distant future release  
2. Release branch - support preparation of a new production release    
3. Hotfix branch - for fixing severe production defects  

### Feature branch  

```sh 
may branch off: develop  
must merge back to: develop  

# creating a feature branch  
$ git checkout -b myfeature develop  

# push this branch to remote repository
$ git push origin myfeature

# during development
$ git add {changed files}
$ git commit -m "message"
$ git push origin myfeature

# incorporating finished feature on develop  
$ git checkout develop  
$ git merge --no-ff myfeature  
$ git branch -d myfeature  
$ git push origin develop  
```



### Release branch  
may branch off from: develop  
must merge back to: develop and master  
naming convention: release-*  

- Branch off when develop reflects desired state of the new release.  
- This branch will be deployed on QA, STAG and then production.  
- No new feature should be added to this branch. Only fixes for bugs found in QAT, UAT.  

### Creating a release branch  
```sh
# see previous release versions (release tags) to decide the current release version  
$ git tag -l (based on the latest tag, create a new major, minor release branch below)    
$ git checkout -b release-1.2.0 develop  

$ ./bump-version.sh 1.2.0
Files modified successfully, version bumped to 1.2.0

$ git commit -a -m "Bumped version number to 1.2.0"
[release-1.2.0 74d9424] Bumped version number to 1.2.0
1 files changed, 1 insertions(+), 1 deletions(-)

# if required by other team members, push this to remote server (for others to work on it, will be deleted later)  
# otherwise, this step can be skipped
$ git push origin release-1.2.0  
- fix minor bugs found during QAT, UAT   
- no new feature  
```

### Finishing a release branch
```sh
$ git checkout master  
$ git merge --no-ff release-1.2.0 -m "merged branch 1.2.0"  
Merge made by recursive.
(Summary of changes)

$ git tag -a 1.2.0 -m 'version 1.2.0'  
push tags to remote server:
$ git push --tags  
$ git push origin master  
```

### Merge changes back to develop  
```sh
$ git checkout develop  
$ git merge --no-ff release-1.2 -m "merged branch 1.2"   
Merge made by recursive.
(Summary of changes)

$ git push origin develop  

remove the release branch from remote
$ git push origin :release-1.2  

remove the release branch (local)   
$ git branch -d release-1.2  

Deploy tag 1.2 on server  
```

# Hot Fix  
```sh
includes quick production fix
branch off from: master  
merge back to master and develop    
```

### Creating hotfix branch  
```sh
$ git checkout master  
# find out the current release tag   
$ git tag -l  
# create a hotfix version name based on the latest tag above (e.g. 1.2 for below naming convention)  
$ git checkout -b hotfix-1.2.1 master  
$ ./bump-version.sh 1.2.1    
Files modified successfully, version bumped to 1.2.1.
$ git commit -a -m "Bumped version number to 1.2.1"  
[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1  
1 files changed, 1 insertions(+), 1 deletions(-)  

# Checkin the fix  
$ git add {files changed}  
# commit the fix  
$ git commit -m "Fixed severe production problem" 
 
# Merging changes back to master
$ git checkout master  
$ git merge --no-ff hotfix-1.2.1  
Merge made by recursive.
(Summary of changes)
 
# Creating tag
$ git tag -a 1.2.1  
# push tags and changes to remote server:
$ git push --tags  
$ git push origin master  

# Merge the changes to develop branch and delete hotfix branch    

$ git checkout develop  
$ git merge --no-ff hotfix-1.2.1  
Merge made by recursive.
(Summary of changes)
$ git push origin develop  
# delete the temporary branch    
$ git branch -d hotfix-1.2.1   


# Note: The one exception to the rule here is that, when a release branch currently exists,   
# the hotfix changes need to be merged into that release branch, instead of develop.

```

### Ref: 
* [http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)  
* [https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)
