
DFIR Project
===========

Abstract
--------
      Incident response team usually working on the offline data,collecting the evidence 
      then analyze data with their own tools, this is the structure for how to build your own data analysis 
      using open source tools .

![alt text](https://github.com/Maboalenen/DFIR/blob/main/DFIR.jpg?raw=true)

Get Starting 
-------
> Building your own DFIR VM <a href='https://drive.google.com/file/d/1y512lcEYH_STG5CN4yU6gsTuK-J947jo/view?usp=sharing' target='_blank'>Doc_DFIR_Download</a>  
> Download DFIR VM <a href='https://drive.google.com/file/d/1n3DNlltoPOFFeV418fbhizm6M47prKbS/view?usp=sharing' target='_blank'>DFIR_Download</a>   
> VM_IP_Address: 192.168.60.133  
> User: elk   
> Password: elk-dfir  

Type of indexing data 
--------------
|output_logs|Ext|
|--|--|
|IIS Exchange  |Log|
|Log2timeline |CSV|
|KAPE|JSON|
|KAPE Windows event logs |JSON|
| Windows Event Logs|EVTX|
|Volatility|CSV|

TOOLS
--------
 <a href='https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.0-amd64.deb' target='_blank'>Elasticsearch</a>  
 <a href='https://artifacts.elastic.co/downloads/kibana/kibana-7.10.0-amd64.deb' target='_blank'>Kibana</a>  
 <a href='https://artifacts.elastic.co/downloads/logstash/logstash-7.10.0-amd64.deb' target='_blank'>Logstash</a>    
 <a href='https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.10.0-amd64.deb' target='_blank'>Filebeat</a>  
 <a href='https://artifacts.elastic.co/downloads/beats/winlogbeat/winlogbeat-7.10.0-windows-x86_64.zip' target='_blank'>Winlogbeat</a>   
 <a href='https://suricata-ids.org/download/' target='_blank'>Suricata</a>  
 <a href='https://docs.zeek.org/en/master/install/install.html#id1' target='_blank'>Zeek</a>  
 <a href='https://github.com/volatilityfoundation/volatility/wiki/Installation' target='_blank'>Volatility</a>
 
How to use 
-----------

Send kape output (JSON format)
```bash
 $ scp kape.json elk@192.168.60.133:/logstash/kape/
 ```
 Send kape windows event Logs (JSON Format)
 ```bash
$ scp kape.json elk@192.168.60.133:logstash/winlog/
```

Send output data log2timeline (CSV Format)
```bash
$ scp timeline.csv elk@192.168.60.133:logstash/timeline/
```
Send IIS exchange logs (Log Format)
```bash
$ scp -r /path_to_logs/ elk@192.168.60.133:logstash/iis/
```
Send Window event Logs to elasticsearch (EVTX) 
```bash
PS C:\Program Files\Winlogbeat> .\winlogbeat.exe -c .\winlogbeat.yml -e
``` 
Zeek read PCAP files and send data to elasticsearch. 

**Note**: zeek output must be at path: /logstash/zeek/
```bash
$ /logstash/zeek$ zeek -r file.pcap
```
Suricate read PCAP file and send data to elasticsearch 
 ```bash	            
$ suricata -c /etc/suricata/suricata.yaml   -r file.pcap -l  /logstash/suricata/
```
Continuous reading any pcap files add on /logstash/suricata/
```bash
 $ suricata   -c /etc/suricata/suricata.yaml  --pcap-file-continuous -r /logstash/suricata/    -l /logstash/suricata/
```
### Volatility 
 
  

