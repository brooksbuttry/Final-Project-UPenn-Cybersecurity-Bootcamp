# Network Forensic Analysis Report

## Time Thieves 
- At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:
    - They have set up an Active Directory network.
    - They are constantly watching videos on YouTube.
    - Their IP addresses are somewhere in the range 10.6.12.0/24.
 
Inspect the network traffic to complete the following report: 

1. What is the domain name of the users' custom site?
    - Frank-n-Ted-DC.frank-n-ted.com
2. What is the IP address of the Domain Controller (DC) of the AD network?
    - 10.6.12.12
3. What is the name of the malware downloaded to the 10.6.12.203 machine?
    - june11.dll
4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). 
5. What kind of malware is this classified as?
    - This malware is classified as a Trojan.

---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name: ROTTERDAM-PC
    - IP address: 172.16.4.205
    - MAC address: 00:59:07:b0:63:a4
    
2. What is the username of the Windows user whose computer is infected?
    - matthijs.devries
3. What are the IP addresses used in the actual infection traffic?
    - 172.16.4.205, 185.243.115.84, 166.62.111.64

