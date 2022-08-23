# Network Forensic Analysis Report

## Time Thieves 
- At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:
    - They have set up an Active Directory network.
    - They are constantly watching videos on YouTube.
    - Their IP addresses are somewhere in the range 10.6.12.0/24.
 
Inspect the network traffic to complete the following report: 

1. What is the domain name of the users' custom site?
    - Domain name: Frank-n-Ted-DC.frank-n-ted.com
    - Filter used: ip.addr == 10.6.12.0/24
    - Results:
<img width="626" alt="Screen Shot 2022-08-22 at 6 39 59 PM" src="https://user-images.githubusercontent.com/99222430/186038338-1b05c2e6-9597-4fca-9b55-8c9f1d755dfd.png">


2. What is the IP address of the Domain Controller (DC) of the AD network?
    - IP Address: 10.6.12.12
    - Results:
   <img width="623" alt="Screen Shot 2022-08-22 at 6 42 29 PM" src="https://user-images.githubusercontent.com/99222430/186038465-ade82f0f-e581-4bb5-9467-17b0d6ea1550.png">

   
3. What is the name of the malware downloaded to the 10.6.12.203 machine?
    - Malware file: june11.dll
    - Results: 
<img width="621" alt="Screen Shot 2022-08-22 at 6 43 57 PM" src="https://user-images.githubusercontent.com/99222430/186038602-47e786c2-cde5-491f-8198-7a787b8aba6b.png">

4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). 

5. What kind of malware is this classified as?
    - This malware is classified as a Trojan.
    - It is disguised to like like the common 'googleupdate.exe' but is named 'googleipdate.exe' which is common with Trojan malware.
   
<img width="622" alt="Screen Shot 2022-08-22 at 6 47 56 PM" src="https://user-images.githubusercontent.com/99222430/186038981-019d9a6f-9a5c-4c1f-98ba-5ec2aa678c7c.png">

---

## Vulnerable Windows Machine

1. Find the following information about the infected Windows machine:
    - Host name: ROTTERDAM-PC
    - IP address: 172.16.4.205
    - MAC address: 00:59:07:b0:63:a4
    - Wireshark filter used: ip.src==172.16.4.205 and kerberos.CNameString
    
2. What is the username of the Windows user whose computer is infected?
    - Windows username: matthijs.devries
    - Wireshark filter used: ip.src==172.16.4.205 and kerberos.CNameString
    - Results:

<img width="633" alt="Screen Shot 2022-08-22 at 6 52 50 PM" src="https://user-images.githubusercontent.com/99222430/186039429-65bcfa83-bc43-4bc4-a417-34e3f13f4d0c.png">


3. What are the IP addresses used in the actual infection traffic?
    - 172.16.4.205, 185.243.115.84
    - These two IP addresses were the two with the most packet traffic occuring. Far more than the other IP's in the packet file.
    - Results:
<img width="619" alt="Screen Shot 2022-08-22 at 6 53 42 PM" src="https://user-images.githubusercontent.com/99222430/186039526-4636e794-88e1-424a-91a8-cbabbea2d9d4.png">



