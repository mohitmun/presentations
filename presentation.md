## Becoming Good at command-line


### Session format

* We will follow simple question( or problem) and its answer (or solution)
* While this might sound like particular use-case but we can eventually expand solutions for many other use-cases


---
### Keyboard shortcuts

* Keybindings - (vim/emacs)
* Default Keybindings are emacs. switch to vi using `set -o vi`
* Ctrl + a - move cursor to start of line
* Ctrl + e - move cursor to end of line
* Ctrl + w - delete one word
* option + left arrow/right arrow - move one word left/right
* Ctrl + u = delete leftwards until the beginning of line
* Ctrl + k - kill to end of the line
* Ctrl + l - clear the screen.

---


### Time

#### Date command recognises simple text

```shell
$ date -d '20 mins ago'
Wed Nov  3 00:13:16 IST 2021

$ date -d 'a week ago'
Wed Oct 27 05:03:27 IST 2021

$ date -d '+ 3 days'
Sat Nov  6 00:33:31 IST 2021
```

#### Running command with `timeout`

* If you wish you end command within `N` seconds if it doesn't succeed then `timeout` command allows you to do so


```shell
$ timeout 2 sleep 10
```

---

### Measuring server response time when you don’t have access to server, using [curl timings](https://blog.cloudflare.com/a-question-of-timing/): 

```shell
$ curl 'https://www.shop101.com/O1Server/saas/shippers/shippingPrices/all' -X GET \ 
-w "TTFB: %{time_starttransfer} Total time: %{time_total} \n" -o /dev/null
TTFB: 0.435743 Total time: 0.435918

```


### Finding http errors and it's count

Let's say our nginx log format looks like this

```
127.0.0.1 - - [02/Nov/2021:06:00:10 +0000] [-] "GET /nginx_status HTTP/1.1" 200 113 0.000 - - "-" "localhost:65432" "-" "Go-http-client/1.1" "-" "-" "-" "2" "-", "-" "" "127.0.0.1"
```

```shell
$ awk '{print $10}' /var/log/nginx/access.log |  head -n 1000 | sort | uniq -c | sort -rn
    844 200
     51 304
     44 201
     35 302
     11 502
      5 500
      3 404
      3 400
      3 301
      1 499
```

---

### Executing commands based on output of other command

Let's say you have certain zips files or logs files in directory. and you want to remove/grep/extract those files based on certain conditions

#### Extracting zips files whos name starts with `extractMe`
```shell
$ find .  -name "extractMe*.zip" -exec unzip {} \; 
```
or Using xargs
```shell
$ \ls extractMe*.zip | xargs unzip 
# Notice \
```
---
#### How to remove files of a given type in a given location which are older than 1 day:

```shell
$ find /tmp/excels/ShippingManifests/ -type f -name '*.pdf' -mtime +1 -exec rm {} \
```

#### creating backup of order_export*.csv files

```shell
$ ls order_export*.csv | xargs -Ifile echo  file "bkp_file"
```

#### Finding duplicated files

```shell
$ find .  -type f -name '*' -not -path "./.git/*"   -print0 | xargs -0 md5sum | sort -k 1,1 | uniq --check-chars=32 --all-repeated=separate
```
---


### Text processing

#### All things grep

* Grepping "Status: " and any 3 letters which comes after that  
```shell
$ grep -oP "Status: .{0,3}" logs/application.log
```
* Searching for multiple strings inside a file.
```shell
$ cat requestIds
801a3303-f25d-4bb9-afdf-48b59196f091
53fa29f3-1e64-4f48-a234-3f7dba388897
$ zgrep -f requestIds logs/application-2021-11-01-1.log.gz
```

same thing above can be achieved using `-F`

```shell
$ zgrep -F $'53fa29f3-1e64-4f48-a234-3f7dba388897\n801a3303-f25d-4bb9-afdf-48b59196f091' logs/application-2021-11-01-1.log.gz
```

* Other grep options   
`-w`: matches words, `-i`: ignore case, `-v`: invert match, `-l`: list only files, `-A,B,C`: context around matches

---
#### Awk one liners

```shell
$ seq 10 | awk '{sum+=$1}END {print sum}'
55

$ seq 10 | awk '($1 % 2) == 1{sum+=$1}END {print sum}'
25

$ seq 10 | awk '{sum+=$1}END {print sum/NR}' # AVG
5.5

$ echo "aa 10
        bb 20
        cc 30" | awk '/bb/ { print $2 }'
20

```
---
### Extract anything (and how to create bash functions)

```shell
$ extract () {
  if [ -f $1 ]; then
    case $1 in
      *.tar.bz2)   tar xvjf $1    ;;
      *.tar.gz)    tar xvzf $1    ;;
      *.tar.xz)    tar xvJf $1    ;;
      *.bz2)       bunzip2 $1     ;;
      *.rar)       unrar x $1     ;;
      *.gz)        gunzip $1      ;;
      *.tar)       tar xvf $1     ;;
      *.tgz)       tar xvzf $1    ;;
      *.zip)       unzip $1       ;;
      *.Z)         uncompress $1  ;;
      *.7z)        7z x $1        ;;
      *.xz)        unxz $1        ;;
      *)           echo "'$1': unrecognized file compression" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}

```
---
#### Once a bash function is defined how can I retrive it's defination?

```shell
$ type extract
extract is a shell function from /Users/mohitmunjani/.zshrc

$ which extract
[prints previous method defination]
```
---


### Dealing with long running commands/processes

* `tmux` or `screen` (also called multiplexers) can be used to create multiple new shell sessions inside single ssh session  
* Other advantage is session persistance. should be used when running long running command or if you have patchy internet connection
* Use `Ctrl+z` to suspend process/command and then use `fg/bg` command to run same suspended process in background or foreground
* `tmux` also have option `-CC` which is integration mode with iterm2
* `tee` can be used to redirect output to file as well as stdout

---


### Making life simpler and easier

* [`jq`](https://github.com/stedolan/jq) - reading/parsing json
* [`jo`](https://github.com/jpmens/jo) - creating json 
* [`tig`](https://jonas.github.io/tig/) - command line git repo browser
* [`diff-so-fancy`](https://github.com/so-fancy/diff-so-fancy) - Good-lookin' diffs

```shell
$ curl -sSL http://localhost:8081/metrics | jq .version
"4.0.0"

$ jo id=543 name=Mohit
{"id":543,"name":"Mohit"}
```
* Working with multiple branches ? Tired of `git stash/stash  pop ` ?  `git worktree` to the rescue

---


### Learning how to learn

* Understand complex command args using explainshell.com
* Learn to read manuals (`man man`)
* Commands (Identify by `which`, `type`)

---


### QnA

---


### Resources

* Brilliant write up on history of tty - https://www.linusakesson.net/programming/tty/
* Awesome way to learn command args - https://explainshell.com/
* Amazing tutorial on awk - https://earthly.dev/blog/awk-examples/
* https://github.com/jlevy/the-art-of-command-line
* https://unix.stackexchange.com/questions/4023/aliases-vs-functions-vs-scripts 
* https://github.com/learnbyexample/Command-line-text-processing
* https://www.tldp.org/LDP/abs/html/abs-guide.html 
* http://zsh.sourceforge.net/Guide/zshguide.html 
* https://github.com/dylanaraps/pure-bash-bible 
* https://news.ycombinator.com/item?id=14634964 
* https://tldp.org/LDP/abs/html/sedawk.html 
* https://www.tldp.org/LDP/abs/html/contributed-scripts.html


---
<img src="https://gitlab.com/O1Dev/O1Server/uploads/dfda55bf7504402118288937932869fa/image.png" width="750" >
