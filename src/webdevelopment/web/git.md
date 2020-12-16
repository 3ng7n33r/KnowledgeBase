# git

## gitupload - add, commit, push
.bashrc:

> if [ -f ~/.bash_aliases ]; then
>     . ~/.bash_aliases 
>     fi

.bash aliases:

> alias gitupload='_gitupload(){ git add . ; git commit . -m "$1" ; git
> push; }; _gitupload'

execute in folder:

    $ gitupload "commit message"

## Branches
**Create local branch from current state**

    $ git checkout -b *branchname*

```bash
// delete branch locally
git branch -d localBranchName

// delete branch remotely
git push origin --delete remoteBranchName
```

## Add submodule to legacy repo

 1. Create github repo
 2. clone
 3. copy paste files and folders into repo
 4. push from sub
 5. cd into master
 6. `$ git submodule add git@github.com:username/reponame.git`


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDczNzQ0MDJdfQ==
-->