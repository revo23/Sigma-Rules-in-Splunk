# Sigma-Rules-in-Splunk

<img width="2337" height="1050" alt="image" src="https://github.com/user-attachments/assets/88a95bd6-1806-4aee-93ca-631ca599083d" />

**Overview**

1. Convert Sigma rules into Splunk queries
2. Ingest Sysmon logs from PC into Splunk
3. Test out sample rule (7Zip Compressing Dump Files) to see if it triggers

**High Level Concept (HLC)**

“Sigma is for log files what Snort is for network traffic and YARA is for files.”

**Solution**

It is built as an open-source standard so that a single Sigma detection rule can be used across the entire security landscape of SIEMs (Eg. Splunk, Azure Sentinel and more), logging platforms (Eg. greylog) and databases (Eg. MySQL).

**Repos**

https://github.com/SigmaHQ/sigma -> main repo containing the rules  
https://github.com/SigmaHQ/pySigma -> python library that parses and converts Sigma rules into queries  
https://github.com/SigmaHQ/sigma-cli -> converter powered by pySigma  

**Apps**  
Vscode - https://code.visualstudio.com/  
Splunk Enterprise Free - https://www.splunk.com/en_us/download/splunk-enterprise.html  
Sysmon - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon  
Sysmon config file - https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml

**Steps**

1. Install Sigma Command Line Interface  

pip3 install sigma-cli  
<br>

2. To list all available backend plugins (Sigma backends are the "drivers" of the Sigma conversion process, and implements the conversion capability that converts each Sigma rule file into a SIEM compatible query.)  

sigma plugin list --plugin-type backend  

<img width="987" height="348" alt="image" src="https://github.com/user-attachments/assets/c409c3af-493f-4ef3-b633-efc4589a1d6e" />

<img width="500" height="1000" alt="image" src="https://github.com/user-attachments/assets/8309e509-e721-49e7-ae38-2bc33a49d486" />
<br>

3. Install splunk backend

sigma plugin install splunk   

<img width="947" height="67" alt="image" src="https://github.com/user-attachments/assets/ab3275e5-1999-4af7-a2f3-ad6e0abd1ed2" />  

backend = splunk  
processing pipeline = sysmon  
Sysmon (System Monitor) is a Windows system service and driver from Microsoft’s Sysinternals Suite. It logs detailed system activity to the Windows Event Log, giving visibility into: Process creation and termination, Network connections, File creation time changes, Registry modifications, Image loading, etc.  
<br>

4. To list all available pipelines plugins 

sigma plugin list --plugin-type pipeline  

<img width="1003" height="205" alt="image" src="https://github.com/user-attachments/assets/a283f973-c82e-4223-ba52-b8795c0fb20c" />  
<br>
<br>

5. Install sysmon pipeline (since we are converting process creation sigma rules)  

sigma plugin install sysmon  

<img width="922" height="88" alt="image" src="https://github.com/user-attachments/assets/68158fa8-9f8c-4a15-a600-a556b6886e11" />    
<br>
<br>

6. To list all available splunk output formats  

sigma list formats splunk  

<img width="860" height="138" alt="image" src="https://github.com/user-attachments/assets/2a011a0a-3016-4d46-8f53-77707889dcb7" />
<br>
<br>

7. Conversion invocation

sigma convert -t splunk -f savedsearches -p sysmon -o savedsearches.conf sigma/rules/windows/process_creation

<img width="1425" height="42" alt="image" src="https://github.com/user-attachments/assets/523375b4-5349-4648-b62b-3bb3225bcc50" />  

an output file can be specified with -o
format specified for conversion with the -f option
<br>
<br>

8. Download Splunk Add-on for Sysmon  
<img width="1362" height="465" alt="image" src="https://github.com/user-attachments/assets/c09ab410-821c-4333-8eef-4c7b545a1498" />

9. Manage Apps > Install App from File (.tgz)  
 
10. Download and Install Splunk Universal Forwarder on localhost 

The Splunk Universal Forwarder (UF) is a lightweight agent that collects and sends data to a Splunk indexer or heavy forwarder for indexing and analysis. It’s designed to efficiently move data from servers, endpoints, or applications to your main Splunk environment without using much system resources. In our case both the forwarder and receiver (indexer) is on the same PC localhost
| Role                   | What it Does                                         | Typical Location                       |
| ---------------------- | ---------------------------------------------------- | -------------------------------------- |
| **Forwarder**          | **Sends data** to another Splunk instance            | On source systems (servers, endpoints) |
| **Receiver (Indexer)** | **Receives data** from forwarders and **indexes** it | On central Splunk servers              |

<img width="373" height="637" alt="image" src="https://github.com/user-attachments/assets/3f358880-f033-4b8b-a617-fbef598a3e83" />

 
12. Setting > Data > Forwarding and receiving  
Add receiver > Listen on port 9997
<img width="1630" height="297" alt="image" src="https://github.com/user-attachments/assets/f69c4a0d-18e7-44dd-bad4-f27303e4e3da" />

13. Add forwarder  127.0.0.1 on port 8089  
<img width="1709" height="280" alt="image" src="https://github.com/user-attachments/assets/482b1656-6fc6-44db-8601-351f18716c91" />

14. Setting > Agent Management
<img width="1881" height="439" alt="image" src="https://github.com/user-attachments/assets/d41681d2-411f-4f23-b075-4cb1d28d1145" />

15. Download and install Sysmon and Sysmon config file with admin powershell
<img width="693" height="313" alt="image" src="https://github.com/user-attachments/assets/96bfa7e8-6be5-42f5-a34e-d65d30eecd06" />

<img width="776" height="578" alt="image" src="https://github.com/user-attachments/assets/ed73bcbb-7cfe-4d54-9fab-1a5d71de197b" />  

16. Verify in Services Sysmon64 is running
<img width="1103" height="600" alt="image" src="https://github.com/user-attachments/assets/dc03c584-1cc5-4f42-bd65-e5642efe3c62" />

17. Verify in Event Viewer Sysmon logs are present  
Applications and Services Logs/Microsoft/Windows/Sysmon/Operational  
<img width="1200" height="1030" alt="image" src="https://github.com/user-attachments/assets/ee228d9c-7a2d-4cc5-9640-3336aa0c69e6" />  

18. Configure Universal Forwarder to Collect Sysmon Logs  (paste into inputs.conf)  
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```
[WinEventLog://Microsoft-Windows-Sysmon/Operational]  
index = sysmon  
disabled = 0
```  

19. Restart Universal Forwarder  
<img width="1385" height="443" alt="image" src="https://github.com/user-attachments/assets/d2393808-bf56-481d-b84f-a40b93944954" />  

20. Sysmon logs seen in Splunk  
<img width="1675" height="898" alt="image" src="https://github.com/user-attachments/assets/58154ee0-e454-412b-90d8-b1b0e0023a6a" />



**References**

https://sigmahq.io/docs/guide/getting-started.html  
https://www.youtube.com/watch?v=WI3C3stHtcI  
https://www.youtube.com/watch?v=gtVQVgkInwk  
https://www.youtube.com/watch?v=rs6q28xUd-o  
https://www.youtube.com/watch?v=uJ7pv6blyog
