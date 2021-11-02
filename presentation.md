## Becoming Good at command-line

### Session format

* We will follow simple question( or problem) and its answer (or solution)
* While this might sound like particular use-case but we can eventually expand solutions for many other use-cases

---

### Measuring server response time

* Using application server logs:
* Using nginx logs: 
```
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c
```
* When you don’t have access to server, using [curl timings](https://blog.cloudflare.com/a-question-of-timing/): 
```
curl 'https://www.shop101.com/O1Server/saas/shippers/shippingPrices/all' -X GET -w "TTFB: %{time_starttransfer} Total time: %{time_total} \n" -o /dev/null
```
---

### Executing commands based on output of other command

Let's say you have certain zips files or logs files in directory. and you want to remove/grep/extract those files based on certain conditions

#### Extracting zips files whos name starts with `extractMe`
```
find .  -name "extractMe*.zip" -exec unzip {} \; 
```
or Using xargs
```
\ls extractMe*.zip | xargs unzip 
# Notice \
```

#### creating backup of order_export*.csv files
```
ls order_export*.csv | xargs -Ifile echo  file "bkp_file"
```

#### Finding duplicated files

```
find .  -type f -name '*' -not -path "./.git/*"   -print0 | xargs -0 md5sum | sort -k 1,1 | uniq --check-chars=32 --all-repeated=separate
```
---

### Time

#### date command recognises simple text

```
date -d '20 mins ago'
date -d 'a week ago'
date -d '+ 3 days'
```

#### Running command with timeout

* If you wish you end command within `N` seconds if it doesn't succeed then `timeout` command allows you to do so
`timeout 2 sleep 10`

#### 
---
### Git

#### Reviewing refactoring changes in a snap

* [TODO Add technique mentioned here] https://gitlab.com/O1Dev/O1Server/-/merge_requests/7065#note_653530489
* https://gitlab.com/O1Dev/O1Server/uploads/2b6531588c9e3745e761e1938035b254/image.png
git config color.diff.oldMoved "red reverse", git config color.diff.newMoved "green reverse", git diff --cached --color-moved=plain

#### Find piece of code which was once part of repo but not now
```
git rev-list --all | xargs git grep MDC | head
```

#### 
---

### Text processing

#### All things grep

* Grepping "Status: " and any 3 letters which comes after that  
```
grep -oP "Status: .{0,3}" logs/application.log
```
* Searching for multiple strings inside a file.
```
~$ cat requestIds
801a3303-f25d-4bb9-afdf-48b59196f091
53fa29f3-1e64-4f48-a234-3f7dba388897
~$ zgrep -f requestIds logs/application-2021-11-01-1.log.gz
```

same thing above can be achieved using `-F`

```
zgrep -F $'53fa29f3-1e64-4f48-a234-3f7dba388897\n801a3303-f25d-4bb9-afdf-48b59196f091' logs/application-2021-11-01-1.log.gz
```

* Other grep options   
`-w`: matches words, `-i`: ignore case, `-v`: invert match, `-l`: list only files, `-A,B,C`: context around matches

#### Awk one liners

```
seq 10 | awk '{sum+=$1}END {print sum}'
seq 10 | awk '($1 % 2) == 1{sum+=$1}END {print sum}'
seq 10 | awk '{sum+=$1}END {print sum/NR}' # AVG
Seq 10 | awk "{print} /7/ {exit}"

sed 's/unix/linux/' geekfile.txt
```

### Extract anything (and how to create bash functions)

```
extract () {
  if [ -f $1 ]; then
    case $1 in
      *.tar.bz2)   tar xvjf $1    ;;
      *.tar.gz)    tar xvzf $1    ;;
      *.tar.xz)    tar xvJf $1    ;;
      *.bz2)       bunzip2 $1     ;;
      *.rar)       unrar x $1     ;;
      *.gz)        gunzip $1      ;;
      *.tar)       tar xvf $1     ;;
      *.tbz2)      tar xvjf $1    ;;
      *.tgz)       tar xvzf $1    ;;
      *.zip)       unzip $1       ;;
      *.Z)         uncompress $1  ;;
      *.7z)        7z x $1        ;;
      *.xz)        unxz $1        ;;
      *.exe)       cabextract $1  ;;
      *.ace)       unace $1       ;;
      *.arj)       unarj $1       ;;
      *)           echo "'$1': unrecognized file compression" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}
```
---


### Dealing with long running commands/processes

* `tmux` or `screen` (also called multiplexers) can be used to create multiple new shell sessions inside single ssh session  
* Other advantage is session persistance. should be used when running long running command or if you have patchy internet connection
* Use `Ctrl+z` to suspend process/command and then use `fg/bg` command to run same suspended process in background or foreground
* `tmux` also have option `-CC` which is integration mode with iterm2
* `tee` can be used to redirect output to file as well as stdout

---


### Keyboard shortcuts

* Keybindings - (vim/emacs)
* Alt + b, Alt + f - move forward/backward by word
* Ctrl + a - move cursor to start of line
* Ctrl + e - move cursor to end of line
* Ctrl + w - delete one word
* option + left arrow/right arrow - move one work left/right
* Ctrl + u = delete leftwards until the beginning of line
* Ctrl + k - kill to end of the line
* Ctrl + l - clear the screen.

---


### Making life simpler and easier

* [`jq`](https://github.com/stedolan/jq) - reading/parsing json
* [`jo`](https://github.com/jpmens/jo) - creating json 
* [`tig`](https://jonas.github.io/tig/) - command line git repo browser
* [`diff-so-fancy`](https://github.com/so-fancy/diff-so-fancy) - Good-lookin' diffs

```
curl -sSL http://localhost:8081/metrics | jq .version
jo id=543 name=Mohit
```
* Working with multiple branches ? Tired of `git stash/stash  pop ` ?  `git worktree` to the rescue

---


### Learning how to learn [This should be second last section]

* Understand complex command args using explainshell.com
* Learn to read manuals (`man man`)
* Commands (Identify by `which`, `type`)

---


### QnA

---


### Resources

* Awesome way to learn command args - https://explainshell.com/
* https://github.com/jlevy/the-art-of-command-line
* https://unix.stackexchange.com/questions/4023/aliases-vs-functions-vs-scripts 
* https://github.com/learnbyexample/Command-line-text-processing
* https://www.tldp.org/LDP/abs/html/abs-guide.html 
* http://zsh.sourceforge.net/Guide/zshguide.html 
* https://github.com/dylanaraps/pure-bash-bible 
* https://news.ycombinator.com/item?id=14634964 
* https://tldp.org/LDP/abs/html/sedawk.html 
* https://www.tldp.org/LDP/abs/html/contributed-scripts.html
