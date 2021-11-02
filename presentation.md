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

* Extracting zips files whos name starts with `extractMe`
```
find .  -name "extractMe*.zip" -exec unzip {} \; 
```
or Using xargs
```
\ls extractMe*.zip | xargs unzip 
# Notice \
```

* creating backup of order_export*.csv files
```
ls order_export*.csv | xargs -Ifile echo  file "bkp_file"
```

___

### Reviewing refactoring changes in a snap

* [Add technique mentioned here] https://gitlab.com/O1Dev/O1Server/-/merge_requests/7065#note_653530489
* https://gitlab.com/O1Dev/O1Server/uploads/2b6531588c9e3745e761e1938035b254/image.png

___

### All things grep

* Grepping "Status: " and any 3 letters which comes after that  
```
grep -oP "Status: .{0,3}" logs/application.log
```
* [Add more problems involving following args]  grep: -F, -w, -i, -v, -l, -A,B,C equivalent for chars

___


### Dealing with long running commands/processes

* `tmux` or `screen` (also called multiplexers) can be used to create multiple new shell sessions inside single ssh session  
* Other advantage is session persistance. should be used when running long running command or if you have patchy internet connection
* Use `Ctrl+z` to suspend process/command and then use `fg/bg` command to run same suspended process in background or foreground
* `tmux` also have option `-CC` which is integration mode with iterm2

* [TODO - Add examples]

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

* `jq` - reading/parsing json

* [`jo`](https://github.com/jpmens/jo) - creating json 
* `tig` - command line git repo browser

```
curl -sSL http://localhost:8081/metrics | jq .version
jo id=543 name=Mohit
```


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
