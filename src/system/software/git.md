# Git

### Squash rebase etc.
[here](https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history)

git init /make a directory a git

git add /add to staging

git commit -m "comment" /commit staged files, -m skips editor and writs comment directly --amend(alter most recent commit, add files to stage if you want to change/include them)

git diff (show uncommitted changes)

 

git status

git log -w --oneline -p /shows history -w (whitespace) -oneline summarizes (works also for show) -p (patch) shows details, --graph shows branches neatly, --all shows all branches)

  

git show

git diff (shows difference of uncommitted files)

  

git tag -a ….. ,,, (-a=annotated (more info), … tag identifier, ,,,, SHA identifier (w/o it tags most recent), -d = delete,-D override delete)

  

git branch name (creates branch name, w/o name command lists branches, -d flag deletes branch)

git checkout name (switch to branch name, creates new branch with -b flag)

git merge name (merges the name branch)

  

git revert SHA (reverse something a commit has done by creating a new commit reversing it)

git reset (deletes commits and their content, HEAD~1 or HEAD^ resets last commit to working directory (--soft toStage --hard to trash))

git reflog (undeletes git reset commands)

  
  
  

Globbing lets you use special characters to match patterns/characters. In the .gitignore file, you can use the following:

-   blank lines can be used for spacing
    
-   \# - marks line as a comment
    
-   * - matches 0 or more characters
    
-   ? - matches 1 character
    
-   [abc] - matches a, b, _or_ c
    
-   ** - matches nested directories - a/**/z matches
    

-   a/z
    
-   a/b/z
    
-   a/b/c/z
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MDQ4NzcyNDYsNDA4NjYxNTU0XX0=
-->