Script to open new geake tab and rename it:

[Lint for the code below](https://askubuntu.com/questions/49715/script-for-opening-tabs-in-guake-terminal)

```
guake --rename-current-tab="tab0" --execute-command="ls" & 
sleep 1 && guake --new-tab="my/path" --rename-current-tab="tab1" --execute-command="ls" &
sleep 2 && guake --new-tab="my/path" --rename-current-tab="tab2" --execute-command="ls" &
exit 0
```

```
guake --rename-current-tab="tab0" --execute-command="ls" & 
sleep 1 && guake --new-tab="/home/alex/rails_work/workspace/.." --rename-current-tab="tab_name_1" --execute-command="ls" & 
sleep 2 && guake --new-tab="/home/alex/rails_work/workspace/.." --rename-current-tab="tab_name_2" --execute-command="ls" &
exit 0
```
