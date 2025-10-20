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


**Steps**

1. Install Sigma Command Line Interface  

pip3 install sigma-cli


2. To list all available backend plugins (Sigma backends are the "drivers" of the Sigma conversion process, and implements the conversion capability that converts each Sigma rule file into a SIEM compatible query.)  

sigma plugin list --plugin-type backend  

<img width="987" height="348" alt="image" src="https://github.com/user-attachments/assets/c409c3af-493f-4ef3-b633-efc4589a1d6e" />

<img width="500" height="1000" alt="image" src="https://github.com/user-attachments/assets/8309e509-e721-49e7-ae38-2bc33a49d486" />


3. Install splunk backend 

<img width="947" height="67" alt="image" src="https://github.com/user-attachments/assets/ab3275e5-1999-4af7-a2f3-ad6e0abd1ed2" />  

backend = splunk  
processing pipeline = sysmon  
Sysmon (System Monitor) is a Windows system service and driver from Microsoft’s Sysinternals Suite. It logs detailed system activity to the Windows Event Log, giving visibility into: Process creation and termination, Network connections, File creation time changes, Registry modifications, Image loading, etc.  


4. To list all available pipelines plugins 

sigma plugin list --plugin-type pipeline

<img width="1003" height="205" alt="image" src="https://github.com/user-attachments/assets/a283f973-c82e-4223-ba52-b8795c0fb20c" />  


5. install sysmon pipeline

<img width="922" height="88" alt="image" src="https://github.com/user-attachments/assets/68158fa8-9f8c-4a15-a600-a556b6886e11" />  

6. conversion invocation

sigma convert -t splunk -f savedsearches -p sysmon -o savedsearches.conf sigma/rules/windows/process_creation

<img width="1425" height="42" alt="image" src="https://github.com/user-attachments/assets/523375b4-5349-4648-b62b-3bb3225bcc50" />  

an output file can be specified with -o
format specified for conversion with the -f option




**References**

https://sigmahq.io/docs/guide/getting-started.html  
https://www.youtube.com/watch?v=WI3C3stHtcI  
