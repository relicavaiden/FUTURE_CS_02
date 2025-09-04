# FUTURE_CS_02
Using Kibana to look at the logs to look at any possible malicious activity. I upload the logs and then I use the Lens to visualize the data; first because there are incorrect tags on the logs, I would need to label all of the necessary tags. 

 

Now that all the fields have been labeled, I want to choose how I want to visualize my data first I am take the Unique count of users as well as the actions to see who has the most accessed. 

 

 

This does not give me much info as each user seems to evenly use the system, now that I see the use case of each user I want to view the “actions” that have taken place. Here I am extracting all the actions attached to the username. And I visually see each user that has malware on their system. 
 
 

This demonstrates a greater view of what is going on within the system but I cannot completely see what the “malware” is so I need to dive a bit further and see what I can discover. So, the next step would be to investigate the “action” and see what the logs are saying exactly that may have been parsed out. 

 

 

Looking under the “action” I use the Explore in Discover to view more of what log is saying in detail.  

 

Here I see that the user Charlie has a trojan on their device. I am looking for more understanding to how and when he gets this Trojan so I filter out all other records and look at Charlie’s timeline of events. 

 

 

Looking at the timeline I see that there was an initial login that failed followed by a successful login and then three connection attempts before the malware was detected.  
 

Looking at the other user Bob I see that there are three malware markings within his system. 
 
 

 

 

Bob’s logs mirror some of what we have seen within Charlie’s logs where there are failed connection attempts before finally being able to access the files within the system. 

 

 

I want to see if other users have been infected so I move my attention to Alice; here I am seeing a different pattern that shows me that there is a threat actor, and they have moved laterally through the system to infect some of the other users as Alice. Alice seems to have the worst of the malware that has been seen so far as there is a rootkit. 
 
 

 

 

 

Alice’s logs show that there was an infection before the attempts to log in were made. 
 

Eve’s user logs show the same as Alice’s with three detections of malware and then successful login attempt before accessing a file. 
 
 

 

 

 

The last user that we have not looked at is Daivd and his user access seems to mirror the access logs of Charlie; where we see only one malware detected but several login attempts. This would state that there are only two possible avenues of compromise. 
 
 

 

 

Now that all users have been viewed here is a breakdown along with an Alert classification and remediation steps 
 
ALERT CLASSIFICATION SUMMARY 

1. MALWARE DETECTION ALERTS 

Severity: Critical 
 Indicators: 

Multiple users and IPs infected. 

Various malware types: Trojan, Rootkit, Spyware, Worm, Ransomware. 

Threat Type 

IP Address 

Affected User(s) 

Timestamp(s) 

Trojan Detected 

10.0.0.5 

bob, eve 

05:48:14, 07:51:14 

Trojan Detected 

192.168.1.101 

eve, alice, charlie 

05:30:14, 04:29:14, 05:49:14 

Trojan Detected 

172.16.0.3 

david, charlie 

05:45:14, 07:45:14 

Trojan Detected 

203.0.113.77 

eve 

05:42:14 

Rootkit Signature 

198.51.100.42 

alice 

04:19:14 

Rootkit Signature 

10.0.0.5 

eve 

07:51:14 

Spyware Alert 

172.16.0.3 

alice 

04:41:14 

Worm Infection 

203.0.113.77 

bob 

05:06:14 

Ransomware Behavior 

172.16.0.3 

bob 

09:10:14 

 

 

 

 

 

Alert Classification: 

Malware outbreak across multiple internal segments and users. 

Infections across all major subnets: 10.x, 172.16.x, 192.168.x, public IPs. 

 

2. SUSPICIOUS LATERAL MOVEMENT / ACCESS PATTERNS 

Severity: High 
 Indicators: 

Users accessing multiple subnets/IPs unusually. 

Access and file interaction on machines flagged for malware. 

User 

IP Addresses Accessed 

Suspicious Behavior 

bob 

10.0.0.5, 172.16.0.3, 198.51.100.42, 203.0.113.77, 192.168.1.101 

File access, malware presence, login success/failure 

charlie 

172.16.0.3, 192.168.1.101, 198.51.100.42, 203.0.113.77 

Frequent connection attempts to compromised hosts 

david 

10.0.0.5, 203.0.113.77, 172.16.0.3, 198.51.100.42 

Multiple login failures/success, file access 

Alert Classification: 

Potential lateral movement 

Use of infected hosts as pivots 

 

3. BRUTE FORCE / CREDENTIAL ATTACK INDICATORS 

Severity: Medium 
 Indicators: 

Multiple login failures, especially followed by success or file access. 

User 

IP Address 

Action 

Timestamp(s) 

charlie 

198.51.100.42 

login failed 

04:23:14 

david 

203.0.113.77 

login failed x2 

08:00:14, 09:02:14 

bob 

172.16.0.3 

login failed 

04:23:14 

bob 

10.0.0.5 

login failed 

04:47:14 

alice 

203.0.113.77 

login failed 

07:02:14 

Alert Classification: 

Brute force or credential stuffing attempt 

Especially concerning due to eventual login success in some cases 

 

4. HIGH-RISK FILE ACCESS ON INFECTED HOSTS 

Severity: High 
 Indicators: 

Access to hosts already infected with malware. 

Access by multiple users, increasing exposure risk. 

IP Address 

Accessed By 

Notes 

10.0.0.5 

bob, david, eve 

Infected (Trojan, Rootkit) 

172.16.0.3 

bob, eve, charlie 

Infected (Trojan, Ransomware) 

203.0.113.77 

alice, david, bob, eve, charlie 

Infected (Trojan, Worm) 

198.51.100.42 

bob, alice, david 

Infected (Rootkit) 

Alert Classification: 

Data exposure risk due to interactions with compromised machines 

 

OVERALL SECURITY POSTURE: COMPROMISED 

Recommended Actions: 

Immediate containment of infected hosts: 

Quarantine: 10.0.0.5, 192.168.1.101, 172.16.0.3, 203.0.113.77, 198.51.100.42 

Re-image and deep scan affected systems. 

Force password resets for all involved users: bob, charlie, david, eve, alice 

Investigate lateral movement vectors and session hijacking possibilities. 

Audit account privileges and validate logon events. 

Monitor for data exfiltration or C2 traffic to external IPs. 

 
