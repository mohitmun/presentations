### Becoming Good at command-line

#### Session format

* We will follow simple question( or problem) and its answer (or solution)
* While this might sound like particular use-case but we can eventually expand solutions for many other use-cases

#### How do I monitor resources on server?

* RAM: `free`
* Filesystem: `ls/du`
* Network: `netstat, lsof `
* CPU: `iostat, top`
* Processes: `ps`
* Everything: `htop`
* Disk space usage: `du ( -a, -h, -s (summarize)`


#### Measuring server response time

* Using application server logs:
* Using nginx logs: 
```
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c
```
* When you don’t have access to server: 
```
curl 'https://www.shop101.com/O1Server/saas/shippers/shippingPrices/all' -X GET -w "TTFB: %{time_starttransfer} Total time: %{time_total} \n" -o /dev/null
```

#### Executing commands based on output of other command

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

#### Reviewing refactoring changes in a snap

* [Add technique mentioned here] https://gitlab.com/O1Dev/O1Server/-/merge_requests/7065#note_653530489
* https://gitlab.com/O1Dev/O1Server/uploads/2b6531588c9e3745e761e1938035b254/image.png

#### All things grep

* Grepping "Status: " and any 3 letters which comes after that  
```
grep -oP "Status: .{0,3}" logs/application.log
```
* [Add more problems involving following args]  grep: -F, -w, -i, -v, -l, -A,B,C equivalent for chars

#### Learning how to learn [This should be second last section]

* Understand complex command args using explainshell.com
* Learn to read manuals (man man)
* Commands (Identify by which, type)

#### QnA

#### Resources

* Awesome way to learn command args - https://explainshell.com/
* https://github.com/jlevy/the-art-of-command-line
* https://unix.stackexchange.com/questions/4023/aliases-vs-functions-vs-scripts 
* https://github.com/learnbyexample/Command-line-text-processing
* https://www.tldp.org/LDP/abs/html/abs-guide.html 
* http://zsh.sourceforge.net/Guide/zshguide.html 
* https://github.com/dylanaraps/pure-bash-bible 
* https://news.ycombinator.com/item?id=14634964 
* https://tldp.org/LDP/abs/html/sedawk.html 
