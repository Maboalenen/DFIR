
DFIR_ELK Project
===========

Abstract
--------
     **Incident** Response Team usually working on the offline data, and analyze it with their own tools, 
      DFIR_ELK project customized build of the open-source consisting of Elasticsearch, Logstash,
      Kibana dashboard, Filebeat, Suricata, Zeek,and Volatility,additionally parsing and visualizing
      the critical output data for forensics tool “Kape by Eric Zimmerman” that help IR team for investigating 
      and analyzing the information security

![alt text](https://github.com/Maboalenen/DFIR/blob/main/DFIR_logo.jpg?raw=true)

Get Starting 
-------
  > Building your own DFIR VM <a href='https://drive.google.com/file/d/1y512lcEYH_STG5CN4yU6gsTuK-J947jo/view?usp=sharing' target='_blank'>Doc_DFIR_Download_V1 </a>    
  > Memory Forensics DFIR VMv2 <a href='https://drive.google.com/file/d/1C42253EbtT7-l2oHHKGKWyexxHfrtFMr/view?usp=sharing' target='_blank'>Doc_DFIR_Download_V2 </a>     
  > Download DFIR VM <a href='https://drive.google.com/file/d/1TMpuZ8oiDX4vx2qovT5bGR8Qogb46NyE/view?usp=sharing' target='_blank'>DFIR_Download</a>       
  > VM_IP_Address: 192.168.60.133  
  > Kibana : 192.168.60.133:5601  
  > Elasticsearch : 192.168.60.133:9200   
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
### Volatility plugins
 Pslist print all running processes with th EPROCESS doubly linked list
  ```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 pslist  > /logstash/memory/pslist.csv
```
 Psscan scan physical memory for Eprosses but it’s can identify the terminated processes with unlocaked
  ```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 psscan  > /logstash/memory/pscan.csv
```
Pstree print process list as tree collect the perent relationships (using Eprocess linked list) 
 ```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 pstree  > /logstash/memory/pstree.csv
```
Psxview helps you detect hidden processes by comparing what PsActiveProcessHead contains with what is reported by various other sources of process listings.
 ```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 psxview  > /logstash/memory/psxview.csv
```
Netscan Network artifacts and socket. (it’s helps to discover suspicious network connections)
```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 netscan  > /logstash/memory/netscan.csv
```
FileScan search for file object in memory and Identifies file in memory even if there are no handled (closed file) finds NTFS special files (such as $MFT) that are not present in VAD tree or process handles list.
```bash
$ vol.py -f memdump.mem --profile=Win2016x64_14393 filescan  > /logstash/memory/filescan.csv
```
Strings used to extract English ASCII and Unicode string from data stream
```bash
$ strings -a -td -el  memdump.mem  >  /logstash/memory/strings.csv
```
### Troubleshooting. 
-------------
 - Make sure all the services are running and active  
 ```bash
 $ sudo service elasticsearch status
 $ sudo service elasticsearch stop
 $ sudo service elasticsearch start 
 ```
 if you restart elatsicsearch make sure to restart logstash after  
 ```bash
 $ sudo service logstash restart 
```
