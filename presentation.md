## Becoming Good at command-line

### Session format

* We will follow simple question( or problem) and its answer (or solution)
* While this might sound like particular use-case but we can eventually expand solutions for many other use-cases

### How do I monitor resources on server?

* RAM: `free`
* Filesystem: `ls/du`
* Network: `netstat, lsof `
* CPU: `iostat, top`
* Processes: `ps`
* Everything: `htop`
* Disk space usage: `du ( -a, -h, -s (summarize)`

___

### Measuring server response time

* Using application server logs:
* Using nginx logs: 
```
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c
```
* When you don’t have access to server: 
```
curl 'https://www.shop101.com/O1Server/saas/shippers/shippingPrices/all' -X GET -w "TTFB: %{time_starttransfer} Total time: %{time_total} \n" -o /dev/null
```
___

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
___

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
___
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
___

### Text processing

#### All things grep

* Grepping "Status: " and any 3 letters which comes after that  
```
grep -oP "Status: .{0,3}" logs/application.log
```
* [Add more problems involving following args]  grep: -F, -w, -i, -v, -l, -A,B,C equivalent for chars

#### Awk one liners

```
seq 10 | awk '{sum+=$1}END {print sum}'
seq 10 | awk '($1 % 2) == 1{sum+=$1}END {print sum}'
seq 10 | awk '{sum+=$1}END {print sum/NR}' # AVG
Seq 10 | awk "{print} /7/ {exit}"

sed 's/unix/linux/' geekfile.txt
```




___


### Dealing with long running commands/processes

* `tmux` or `screen` (also called multiplexers) can be used to create multiple new shell sessions inside single ssh session  
* Other advantage is session persistance. should be used when running long running command or if you have patchy internet connection
* Use `Ctrl+z` to suspend process/command and then use `fg/bg` command to run same suspended process in background or foreground
* `tmux` also have option `-CC` which is integration mode with iterm2
* `tee` can be used to redirect output to file as well as stdout

___


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

___


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

___


### Learning how to learn [This should be second last section]

* Understand complex command args using explainshell.com
* Learn to read manuals (man man)
* Commands (Identify by which, type)

___


### QnA

___


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
