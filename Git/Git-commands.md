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
