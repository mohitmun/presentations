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