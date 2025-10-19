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

1. pip3 install sigma-cli

2. splunk plugin (backend) is available, install it

<img width="976" height="320" alt="image" src="https://github.com/user-attachments/assets/568021db-b12b-444a-831e-1348e2818a08" />  
<img width="896" height="61" alt="image" src="https://github.com/user-attachments/assets/6538cd95-0810-4731-89cd-64b03ab05f25" />

sigma convert -t <backend> -p <processing pipeline 1> -p <processing pipeline 2> [...] <directory or file>

backend = splunk
processing pipeline = 

3. install splunk backend (Sigma backends are the "drivers" of the Sigma conversion process, and implements the conversion capability that converts each Sigma rule file into a SIEM compatible query.)

<img width="947" height="67" alt="image" src="https://github.com/user-attachments/assets/ab3275e5-1999-4af7-a2f3-ad6e0abd1ed2" />  





**References**

https://sigmahq.io/docs/guide/getting-started.html  
https://www.youtube.com/watch?v=WI3C3stHtcI  
