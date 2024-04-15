<h1> Incident Report: OS Hardening</h1>

<h2>Description</h2>
In this activity, I will take on the role of a cybersecurity analyst working for a company that hosts the cooking website, yummyrecipesforme.com. Visitors to the website experience a security issue when loading the main webpage. My job is to investigate, identify, document, and recommend a solution to the security problem. 

When investigating the security event, I will review a tcpdump log. I will need to identify the network protocols used to establish the connection between the user and the website. Network protocols are the communication rules and standards networked devices use to transmit data. Unfortunately, malicious actors can also use network protocols to invade and attack private networks. Knowing how to identify the protocols commonly used in attacks will essentially help me protect my organization’s network against these types of security events.
<br />

<h2>Scenario</h2>


I am a cybersecurity analyst for yummyrecipesforme.com, a website that sells recipes and cookbooks. A former employee has decided to lure users to a fake website with malware. 

The baker executed a brute force attack to gain access to the web host. They repeatedly entered several known default passwords for the administrative account until they correctly guessed the right one. After they obtained the login credentials, they were able to access the admin panel and change the website’s source code. They embedded a javascript function in the source code that prompted visitors to download and run a file upon visiting the website. After embedding the malware, the baker changed the password to the administrative account. When customers download the file, they are redirected to a fake version of the website that contains the malware. 

Several hours after the attack, multiple customers emailed yummyrecipesforme’s helpdesk. They complained that the company’s website had prompted them to download a file to access free recipes. The customers claimed that, after running the file, the address of the website changed and their personal computers began running more slowly. 

In response to this incident, the website owner tries to log in to the admin panel but is unable to, so they reach out to the website hosting provider. Myself and other cybersecurity analysts are tasked with investigating this security event.

To address the incident, I first create a sandbox environment to observe the suspicious website behavior. I run the network protocol analyzer tcpdump, then type in the URL for the website, yummyrecipesforme.com. As soon as the website loads, I am prompted to download an executable file to update my browser. I accept the download and allow the file to run. I then observed that my browser redirected me to a different URL, greatrecipesforme.com, which contains the malware.

A senior analyst confirms that the website was compromised. The analyst checks the source code for the website. They noticed that javascript code had been added to prompt website visitors to download an executable file. Analysis of the downloaded file found a script that redirects the visitors’ browsers from yummyrecipesforme.com to greatrecipesforme.com. 

The cybersecurity team reports that the web server was impacted by a brute-force attack. The disgruntled baker was able to guess the password easily because the admin password was still set to the default password. Additionally, there were no controls in place to prevent a brute-force attack. 

My job is to document the incident in detail, including identifying the network protocols used to establish the connection between the user and the website.  I should also recommend a security action to take to prevent brute-force attacks in the future. <br/> <br/>


<h2>tcpdump Traffic Log:</h2>

14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24)
14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)


14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0
14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0
14:18:36.786529 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1
14:18:36.786595 IP yummyrecipesforme.com.http > your.machine.36086: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 0
…<a lot of traffic on the port 80>... 


14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.17 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0
14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0
14:25:29.576524 IP your.machine.56378 > greatrecipesforme.com.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
14:25:29.576590 IP your.machine.56378 > greatrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 73: HTTP: GET / HTTP/1.1
14:25:29.576597 IP greatrecipesforme.com.http > your.machine.56378: Flags [.], ack 74, win 512, options [nop,nop,TS val 3302989649 ecr 3302989649], length 0
…<a lot of traffic on the port 80>... 

The logs show the following process:

- The browser initiates a DNS request: It requests the IP address of the yummyrecipesforme.com URL from the DNS server.

- The DNS replies with the correct IP address. 

- The browser initiates an HTTP request: It requests the yummyrecipesforme.com webpage using the IP address sent by the DNS server.

- The browser initiates the download of the malware.

- The browser initiates a DNS request for greatrecipesforme.com.

- The DNS server responds with the IP address for greatrecipesforme.com.

- The browser initiates an HTTP request to the IP address for greatrecipesforme.com.

<h2>Section 1: Identify the network protocol involved in the incident </h2>

During the investigation, the following network protocols were identified:
During the investigation, the following network protocols were identified:
1.	DNS (Domain Name System): Used for resolving domain names to IP addresses. DNS requests were made to resolve the IP addresses of both yummyrecipesforme.com and greatrecipesforme.com.
2.	HTTP (Hypertext Transfer Protocol): Used for transmitting data over the web. HTTP requests were made to access the webpage for yummyrecipesforme.com and greatrecipesforme.com, as well as to download the malware-infected file.<br />

<h2>Section 2: Document the Incident</h2>

Incident Overview:

Around 2:18 PM, a security incident was detected on the website yummyrecipesforme.com. The incident involved a former employee executing a series of malicious actions to compromise the website’s integrity and distribute malware to visitors.

Incident Timeline:

1.	The attacker initiated a brute force attack to gain unauthorized access to the web host.
2.	Upon successfully obtaining administrative credentials through the brute force attack, the attacker accessed the admin panel and injected malicious JavaScript code into the website’s source code. 
3.	Visitors to the website were prompted to download and execute a file containing malware.
4.	The malware caused visitors’ browsers to redirect to a fake website, greatrecipesforme.com, where additional malicious activities occurred.
Discovery and Analysis:

Discovery and Analysis:

The incident was discovered after multiple customers reported unusual behavior on the website, including prompts to download files and subsequent performance issues on their personal computers. Investigation revealed the presence of injected JavaScript code and analysis of the downloaded file confirmed its malicious nature.  

Root Cause:

The compromise occurred due to the use of default administrative credentials and the lack of controls to prevent brute-force attacks. Additionally, insufficient security measures allowed the attacker to modify the website source code without detection. 
 <br /> <br />  

<h2>Section 3: Recommendations</h2>

Implementing a robust password policy and account lockout mechanism is essential to prevent brute force attacks in the future.

1.	Password Policy Enforcement:<br /> 
•	Enforce Strong Passwords: Require the use of strong, unique passwords for all user accounts, particularly administrative accounts. Strong passwords should include a combination of uppercase and lowercase letters, numbers, and special characters.<br /> 
•	Regular Password Updates: Implement a policy mandating regular password updates to mitigate the risk of compromised credentials.<br /> 
2.	Account Lockout Mechanism:<br /> 
•	Implement an account lockout mechanism that automatically locks the user's account after a specified number of failed login attempts. This measure prevents malicious actors from conducting brute force attacks by limiting the number of login attempts allowed.  
•	Define appropriate thresholds for failed login attempts and specify the duration of account lockout periods to balance security and usability. 

By implementing these security measures, the risk of successful brute force attacks can be significantly reduced, thereby enhancing the overall security posture of yummyrecipesforme.com.

 <!--
 ```diff
- text in red
+ text in green
! text in orange
