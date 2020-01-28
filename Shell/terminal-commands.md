# Terminal commands



## Run multiple terminal commands in one line:

[link_1](https://www.howtogeek.com/269509/how-to-run-two-or-more-terminal-commands-at-once-in-linux/)

* Opt 1:  using semicolon(;)
Will execute the command without checking if the previous command was successfuly executed

```
ls ; pwd ; whoami
```

* Opt 2: using AND (&&)
Will check if the previous command was successfuly executed and then will execute the next one

```
mkdir MyFolder && cd MyFolder
```

* Opt 3: using OR (||)
Will execute the second command if the first command fail

```
[ -d ~/MyFolder ] || mkdir ~/MyFolder
```

## Kill a process script

```
ps -ef | grep your_process_name | grep -v grep | awk '{print $2}' | xargs kill
```
