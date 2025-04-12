[[index|Home]]

[OverTheWire: Wargames](https://overthewire.org/wargames/)

# Ethical Hacking :
[Ethical hacking techniques - DEV Community](https://dev.to/snyk/ethical-hacking-techniques-3anh)
# Types :
[What Are The Five Steps Of Ethical Hacking? - DEV Community](https://dev.to/sudip_sg/what-are-the-five-steps-of-ethical-hacking-4hfe)
1) Social engineering : exploit human psychology, rather than technical security gaps to gain access to data and applications. They trick legitimate users into submitting their passwords or installing malicious software that grants them access to network machines and services.

2) Web application hacking : 
     Vulnerabilities that manipulate the application :
     - [Cross-Site Scripting](https://crashtest-security.com/cross-site-scripting-xss/)
     - [Cross-Site Request Forgery](https://crashtest-security.com/cross-site-request-forgery-csrf/)
     - [Insecure configuration](https://crashtest-security.com/disable-ssl-insecure-algorithm/)
     - [Injection attacks](https://crashtest-security.com/what-are-the-different-types-of-injection-attacks/)

3) Hacking wireless networks 

4) System hacking (hacking personal computer and servers)

 [Crashtest Security](https://crashtest-security.com/) offers a comprehensive suite of testing tools that help you identify threats within your application.


# Methods :
[Ethical hacking techniques - DEV Community](https://dev.to/snyk/ethical-hacking-techniques-3anh)
1) predictive analytics models is one of the main uses of AI and ML in cybersecurity. These models look for trends and abnormalities that can point to a potential security problem by analyzing data from a range of sources, including network traffic, user behaviour, and system logs.
2) Internet of Things testing's objective is to identify any security flaws in IoT hardware, communication protocols, and the networks they use by network mapping, device identification, firmware analysis, penetration testing, and vulnerability scanning.
3) Social engineering attacks, such as phishing and pretexting
4) Red teaming involves simulating a real-world attack scenario to identify potential vulnerabilities and test an organization's incident response capabilities.
5) Bug bounty programs allow organizations to incentivize ethical hackers to identify potential vulnerabilities in their systems and report them in exchange for a reward.

# Steps :
1) Reconnaissance : hacker documents the organization’s request, finds valuable configuration and login information of the system, and probes the networks.
    Informations such as :
    - Naming conventions
    - Services on the network
    - Servers handling workloads in the network
    - IP Addresses
    - Names and Login credentials of users connected to the network
    - Physical location of target machine
    
2) Penetration testing : 
    - Network Mapping : This involves discovering the network topology, including host information, servers, routers, and firewalls within the host network. Once mapped, white hat hackers can visualize and strategize the next steps of the ethical hacking process.
    - Port Scanning : Ethical hackers use automated tools to identify any open ports on the network. This makes it an efficient mechanism to enumerate the services and live systems in a network, and how to establish a connection with these components.
    - Vulnerability Scanning : The use of automated tools to detect weaknesses that can be exploited to orchestrate attacks.
       Tools for scanning : 
       - SNMP Sweepers
       - Ping sweeps
       - Network mappers
       - Vulnerability scanners
       
3) Gaining Access : Attempting to send a malicious payload to the application through the network, an adjacent subnetwork, or physically using a connected computer.
    Tools to simulate attempted unauthorized access,
    - Buffer overflows
    - Phishing  
    - [Injection attacks](https://crashtest-security.com/what-are-the-different-types-of-injection-attacks/)
    - [XML External Entity processing](https://crashtest-security.com/xxe-processing/)
    - Using components with known vulnerabilities.
    
    If the attacks are successful, the hacker has control of the whole or part of the system and may simulate further attacks such as data [breaches](https://crashtest-security.com/prevent-breach-attacks/) and Distributed Denial of Service (DDoS).
    
4) Maintaining Access :  involves processes used to ensure the hacker can access the application for future use. A white-hat hacker continuously exploits the system for further vulnerabilities and [escalates privileges](https://crashtest-security.com/privilege-escalation-guide/) to understand how much control attackers can gain once they get past security clearance. Some attackers may also try to hide their identity by removing any evidence of an attack and installing a backdoor for future access.
    
5) Clearing Tracks : To avoid any evidence that leads back to their malicious activity, hackers perform tasks that erase all traces of their actions.
    This includes : 
    - Uninstalling scripts/applications used to carry out attacks
    - Modifying registry values
    - Clearing logs
    - Deleting folders created during the attack
    For those hackers looking to maintain undetected access, they tend to hide their identity using techniques such as :
    - Tunneling
    - Stenography