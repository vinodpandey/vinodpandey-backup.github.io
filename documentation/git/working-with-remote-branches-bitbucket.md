---
layout: page
title: Working with remote branches (bitbucket)
---

## Initial Repository Setup
* Create a new repository with initial README file
* Create branch develop from master
    Create Branch > Branch Name - develop - Create 

## Development flow 

#### Create feature branch
```
Create a new feature branch from develop (using bitbucket UI)  
Create branch > Branch from - select develop  
Branch name - myfeature > Create 
```

#### Code setup
```
git clone git@bitbucket.org:elpistechsolutionspvtltd/git-flow.git
cd git-flow

git fetch && git checkout myfeature


```
                    
#### Commiting changes to local repository
Amend all changes to a single commit.

```
git add file-name
git commit -m "message"

# append multiple commits to same commit
git commit --amend    
```    


#### Pushing changes to remote repository

```
# pull latest changes from remote repository before pushing changes
git pull --rebase 
```

```
# If there are any remote changes, below message is displayed
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From bitbucket.org:elpistechsolutionspvtltd/git-flow
   c8c07e7..4023785  myfeature  -> origin/myfeature
First, rewinding head to replay your work on top of it...
Applying: message
```

```
# If there are any merge conflicts, below message is displayed
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From bitbucket.org:elpistechsolutionspvtltd/git-flow
   4023785..9396d94  myfeature  -> origin/myfeature
First, rewinding head to replay your work on top of it...
Applying: message
Using index info to reconstruct a base tree...
M	README.md
.git/rebase-apply/patch:9: trailing whitespace.
test 
warning: 1 line adds whitespace errors.
Falling back to patching base and 3-way merge...
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: Failed to merge in the changes.
Patch failed at 0001 message
The copy of the patch that failed is found in: .git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".

# resolve merge conflicts
git mergetool 

# continue rebase
git rebase --continue
Applying: message
```

```
# pushing changes to remote server
git push origin myfeature

```

#### Merging remote commits to single commit

When commits have already been published to remote repository and we want to merge multiple commits to single commit.

```
git log
commit 2a4e0b3cb68361f1b22e0152ccc9c1ac8fedc4ca
Author: Vinod Pandey
Date:   Mon Feb 27 12:58:52 2017 +0530
commit2
commit 31e2be090dac63d527503f6ab34b263774978fa0
Author: Vinod Pandey
Date:   Mon Feb 27 12:58:07 2017 +0530
commit1
    
# for merging last 2 commits in single commit in remote repository    
git rebase -i HEAD~2
# replace pick with squash for second line

git push origin myfeature --force

git log
commit f9eb0d99a4d262bbe62a4b545ed5614b22cdc70b
Author: Vinod Pandey 
Date:   Mon Feb 27 12:58:07 2017 +0530

commit1
    
commit2

commit 2272a6ff7c31b16fdcc3a039bbdb4d0f7d2c5988
Author: Vinod Pandey
Date:   Mon Feb 27 12:43:31 2017 +0530

message

```
