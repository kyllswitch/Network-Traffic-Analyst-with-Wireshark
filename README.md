SOC Analyst Project: Network Traffic Analysis with Wireshark
Capturing and Analyzing HTTP Traffic for Suspicious Activity

Project Overview
Objective: Use Wireshark to capture live network traffic, filter HTTP requests, and identify potential malicious activity.



Why It Matters:
SOC analysts use packet analysis to investigate breaches, malware C2 traffic, and scanning.  Wireshark is the industry-standard tool for deep packet inspection.



Skills Demonstrated:
Packet capturing,
Protocol analysis,
Identifying suspicious HTTP traffic


Skills Gained:
Packet analyst,
Threat Detection,
OSINT correlation




1.  Opening Wireshark on Kali Linux:
	To do this all we’re going to do is open a terminal on Kali Linux and at the command prompt type in:  wireshark
*Note:  Kali Linux already comes with Wireshark by default but if it’s missing then type the command: sudo apt install wireshark




2.  Capturing Traffic:
Once wireshark is open we have to choose a network interface.
eth0 for Ethernet
wlan0 for Wifi
Avoid lo (loopback) unless testing local traffic



<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/53c8ae3a-3007-4606-bdab-15ec360ebd37" />


 

Click on which one works for you (highlight), and click on the blue shark fin icon in the top left corner under the file option.  This is so we can start to capture traffic.
For this project we’re going to generate http traffic.  So on my Ubuntu (virtual machine) I opened up the firefox browser and went to an http website (not https).




3.  Filter for traffic
In the Wireshark filter bar we’re going to type:
http.request.method == "GET"
(See Below)


<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/91fff906-a956-40d4-b1ce-88036a482fa6" />


 

This filters only Http Get requests which are common in web attacks like traversal.  Alternative filters include:
http contains “admin”  	(Finds HTTP requests to admin pages)
http.requests.method == “POST”  	(checks for form submissions/Brute forcing)
ip.src == YOUR_IP	(shows traffic from your machine)




4. Analyze packet Traffic
To do this we’re going to right click on a packet, follow, and select HTTP Stream to see the raw request.  An example of normal traffic would show up as:
GET / HTTP/1.1
Host: example.com
User-Agent: curl/7.68.0 (Example)
In the case of the screenshot (see below) this is the case with the fields filled out with the necessary information that we captured from the Ubuntu VM.



<img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/026b0ee6-f776-4f34-bcf9-e629e9cb83b9" />


 

GET / HTTP/1.1
Host: neverssl.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64;rv :142.0) Gecko/20100101 Firefox/142.0 (Example of real normal traffic)



Suspicious Traffic to look for when analyzing:

Directory Traversal Attacks
GET /../../etc/passwd HTTP/1.1
(An attempt to access sensitive files.)

SQL Injection Patterns
GET /products?id=1’ UNION SELECT username,password FROM users—HTTP/1.1

User-Agent Spoofing
User-Agent: sqlmap/1.7.2
(Indicates automated hacking tools.)




5.  Export Suspicious Packets for Reporting
  Any suspicious packets that you find, select, go to File, and choose Export Specified      Packets.
  Save in a PCAP format for evidence. (Example: suspicious.pcap)






6.  Cross-check Suspicious Ips
  Look at the destination IP of odd requests and check it on one of the two platforms
	AbuseIPDB/
	VirusTotal

Conclusion:
Wireshark is a must-have tool for cybersecurity analysts that helps in detecting web attacks, C2 traffic, and scanning.  It’s always a good idea to correlate these findings with threat intelligence. (AbuseIPDB, VirusTotal)

