# Sigma-Rules-in-Splunk

<img width="2337" height="1050" alt="image" src="https://github.com/user-attachments/assets/88a95bd6-1806-4aee-93ca-631ca599083d" />


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
 
10. Download and Install Splunk Universal Forwarder  
The Splunk Universal Forwarder (UF) is a lightweight agent that collects and sends data to a Splunk indexer or heavy forwarder for indexing and analysis. It’s designed to efficiently move data from servers, endpoints, or applications to your main Splunk environment without using much system resources.
 
11. Setting > Data > Forwarding and receiving  
Listen on port 9997
<img width="1630" height="297" alt="image" src="https://github.com/user-attachments/assets/f69c4a0d-18e7-44dd-bad4-f27303e4e3da" />





**References**

https://sigmahq.io/docs/guide/getting-started.html  
https://www.youtube.com/watch?v=WI3C3stHtcI  
