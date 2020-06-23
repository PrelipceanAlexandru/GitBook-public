# Git commands

## Git reset
```
git reset HEAD~1 --soft    remove last commit locally

git reset                  undo command for git add  
git reset <file>           

git reset --hard 
git reset --hard <commit_number>           You can add commit number 
```

## Undo git add

To undo git add before a commit:

```
git reset <file>
or
git reset to unstage all changes.
```
 
[get a branch up to date with master](https://gist.github.com/santisbon/a1a60db1fb8eecd1beeacd986ae5d3ca)

## Git stash

https://www.freecodecamp.org/news/useful-tricks-you-might-not-know-about-git-stash-e8a9490f0a1a/

## Git log 

```
git log app/api/api_helpers.rb 

git show 29972425b4e0dc7e6731952b7516f6e06df46204
```


## Undo a commit and redo

$ git commit -m "Something terribly misguided"             # (1)
$ git reset HEAD~                                          # (2)
<< edit files as necessary >>                              # (3)
$ git add ...                                              # (4)
$ git commit -c ORIG_HEAD                                  # (5)
This is what you want to undo.
This does nothing to your working tree (the state of your files on disk), but undoes the commit and leaves the changes you committed unstaged (so they'll appear as "Changes not staged for commit" in git status, so you'll need to add them again before committing). If you only want to add more changes to the previous commit, or change the commit message1, you could use git reset --soft HEAD~ instead, which is like git reset HEAD~2 but leaves your existing changes staged.
Make corrections to working tree files.
git add anything that you want to include in your new commit.
Commit the changes, reusing the old commit message. reset copied the old head to .git/ORIG_HEAD; commit with -c ORIG_HEAD will open an editor, which initially contains the log message from the old commit and allows you to edit it. If you do not need to edit the message, you could use the -C option.
Beware, however, that if you have added any new changes to the index, using commit --amend will add them to your previous commit.

If the code is already pushed to your server and you have permissions to overwrite history (rebase) then:

git push origin master --force
You can also look at this answer:
