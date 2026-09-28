# SOC-Automation-Projects
In this project, we are desinging, installing, configuring, and generating actual events on the end point. This project is a starting point for SOC Analysts and other SOC positions. 
This repostiory will be updated whenever a new project idea is presented, starting with the home lab creation to invistigating and analyzing threats and malware similar to what a real SOC Analyst experiences in day to day basis. 


# 1- DESIGN 
This phase will showcase the architecture that the project will follow, the importantce here is to clearly visualize the data flow and interactions between different components. 
This diagram is created using **draw.io** with the following components:
1. Window 10 Client Wazuh Agent - responsible for sending events
2. Router - routing traffic between nodes
3. Internet - Main point of traffic
4. Wazuh Manager - Sending alerts to Shuffle & performing Responsive actions
5. Shuffle - Sending alerts to TheHive & Sending emails
6. TheHive - Recives alerts from Shuffle

Each link with a different color represents a different process, this being:<br />
<br />
1.**White** : The first link in *step 1* connecting Wazuh Agent to the Internet to send events, in which the Internet then sends to Wazuh manager in *step 2*, followed by Wazuh manager sending an alert to Shuffle in *step 3*, and finally in *step 5* where Shuffle sends alerts to TheHive.<br />
<br />
2.**Green** : This link focuses on the interaction of sending and reciving emails from Shuffle to the SOC Analyst, shown in *steps 6 and 7*.<br />
<br />
3.**Orange** : This link is concerned with the sending responsive actions and performing said actions, as shown in *steps 8 and 9*.<br />
<br />
4.**Pink** : The shortest link, yet one of the most important. This link is responsible for the enrichment of IOCS needed for the responsive actions later on. Shown in *step 4*.<br />
<br />
<br />
![Alt text](Design.png)
<br />
<br />
<br />
# 2- INSTALLING 
In this phase, we'll instal three cmain component:<br />
1. Window 10 with *Sysmon*
2. Wazuh Server
3. TheHive Server

## Installing Sysmon 
**Fisrt** : Download Sysmon from  Sysmon — Sysinternals | Microsoft Learn, once downloaded extract the content from the ZIP folder<br />
**Second** : Download a Sysmon xml configuration file, there exist many public files such as SwiftOnSecurity/sysmon-config on Github, Once downloaded, place in the same folder as Sysmon.<br />
**Third** : Install Sysmon using Powershell Administrator by navegating the path of the folder and running the following command and accepting the terms and conditions. 
```
./sysmon.exe -accepteula -i sysmonconfig-export.xml
```
**Fourth** : Confirm Sysmon is running by either:<br />
-Checking Event Viwer<br /> 
-Checking Services<br />
If Sysmon can be found in both or one of these, Sysmon is successfully installed.

## Installing Wazuh Server
Using *DigitalOcean* to create an account, or link Github existing account to start the **Wazuh Server**. Once the account is ready, click **create** and select **droplet**. For an OS, Ubuntu 24.04(LTS)x64, for CPU pick Premium AMD, and for the plan select $48.00/mo (4 vCPU - 8 GB RAM - 160 GB SSD - 5 TB Transfer). Finally create a password or SSH Key (Instructions included in digitalOcean). Before finishing the Droplet, change the name to "Wazuh Server".
<br /> 
Now that the server is created, it's preferable to create a **Firewall** to avoid spams, in the *Networking* section press **Firewall**, create Firewall and for the rules **All TCP** and remove all IPS adding only your personal IP, which can be acquired by searching:<br />
```
Whatismyipaddress
```
Then apply the same process for UDP as well. <br /> 
Once this is done, linking the FW to the Wazuh server is the next step.
Going back to the Server and selecting *Networking*, scrolling to the bottom and selecting *Edit* in the Firewall section, will display every FW created by the user, select the one we created for this project. Finally from the Firewall tab click **Droplets** and select the Wazuh Server then add it. 



