# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- ELK
  - **Operating System**: Ubuntu
  - **Purpose**: The ELK monitoring stack 
  - **IP Address**: 192.168.1.100
- Kali
  - **Operating System**: Debian Kali
  - **Purpose**: Penetration Testing
  - **IP Address**: 192.168.1.90
- Capstone
  - **Operating System**: Ubuntu
  - **Purpose**: Vulnerable Web Server, Filebeat and Metricbeat are installed and will forward logs to ELK machine
  - **IP Address**: 192.168.1.105
- Target
  - **Operating System**: Debian GNU/Linux 8 
  - **Purpose**: Vulnerable WordPress Host
  - **IP Address**: 192.168.1.110


![Screen Shot 2022-08-22 at 7 02 41 PM](https://user-images.githubusercontent.com/99222430/186040347-69ee316d-ceb8-43b3-adb3-0def0067b6ee.png)


### Description of Targets

- The target of this attack was: Target 1 (192.168.1.110). 
- Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

<img width="600" alt="Screen Shot 2022-08-22 at 7 04 52 PM" src="https://user-images.githubusercontent.com/99222430/186040555-c6c7e3d1-ad9b-44b1-aa43-ae97d383f19d.png">


#### Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:
  - **Metric**: WHEN count() GROUPED OVER top 5 'http.response.status_code'
  - **Threshold**: IS ABOVE 400 
  - **Vulnerability Mitigated**: Enumeration / Brute Force
  - **Reliability**: This alert is highly reliable. Filtering out error codes 400 and above filters out the normal activity/successful responses you would see normally. Anything above error code 400 is a cause for concern.

#### HTTP Request Size Monitor
HTTP Request Size Monitor is implemented as follows:
  - **Metric**: WHEN sum() of http.resquest.byte OVER all documents
  - **Threshold**: IS ABOVE 3500
  - **Vulnerability Mitigated**: COde Injection in HTTP requests or DDOS
  - **Reliability**: This alert has the potential to create false positives, therefore I give it a medium reliablity. It is possible there is a large number of legitimate HTTP traffic that happens, so it isn't a "highly" reliable alert as of now.

#### CPU Usage Monitor
CPU Usage Monitor is implemented as follows:
  - **Metric**: WHEN max() OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: IS ABOVE 0.5
  - **Vulnerability Mitigated**: Malicious software, malware/viruses that are running and taking up space 
  - **Reliability**: This alert is highly reliable. This alert can serve as a helper, in the fact that it can show you wehere your CPU can improve its usage even without malicious programs running.
