`docker-compose up`    

access `chronograf` UI at `http://localhost:8888/`  
in the connection page; 
use `172.31.0.2:8086` as the source address(ie address of influxDB).  
`172.31.0.2` is the IP address of the docker container for influxDB.   
leave user and password as empty


It looks like ONLY syslog input (telegraf plugin) can be used for logs currently;   
```
Logs are pulled from the syslog measurement. Other log inputs and alternate log measurement options will be available in future updates.
``` 
-https://docs.influxdata.com/chronograf/v1.6/guides/analyzing-logs/    
I'll discontinue this adventure until more plugins can be used for logs.  

Ideally, atleast the tail plugin should be supported for logs; https://github.com/influxdata/telegraf/tree/master/plugins/inputs/file   
That plugin seems like it currently only work for metrics and not logs  


Use logparser:   
1. https://github.com/influxdata/telegraf/tree/master/plugins/inputs/logparser  
2. https://www.influxdata.com/blog/telegraf-correlate-log-metrics-data-performance-bottlenecks/  

This json log:  
```sh
'{"age": 42, "name": "komuw", "req_id": "qe4252"}'
```
is matched by this logparser:  
```sh
"age": %{NUMBER:age:int}, "name": "%{WORD:name:string}", "req_id": "%{WORD:req_id:string}"
```  
use: https://grokdebug.herokuapp.com/ to test custom parsers
