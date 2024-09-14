# Implementing a Cloud SOC and Honeynet in Azure

## Objective

## Setting up VMs on Azure

I created two Virtual Machines (VMs), Windows (windows-vm) and Linux (linux-vm), making sure that they are in the same region to allow them to be part of the same virtual network. Both machines boast 2 CPUs and some of the lowest prices I could find.
Next, I opened them up so that they accept all traffic coming from the Internet. This can be done in the network security groups of the two machines by creating a rule that allows all traffic. Currently if traffic inbound from the Internet is not RDP, then it is evaluated by the next set of rules which can be seen until it is eventually denied if it doesn't pass them. I edited the Windows machine first. 

<img width="1792" alt="Screenshot 2024-09-12 at 20 40 57" src="https://github.com/user-attachments/assets/a47e8a23-36df-47c0-af05-a0af4ed15d52">
<br>
<br>

To let all traffic in I deleted the RDP rule, and created the new rule:

<img width="1792" alt="Screenshot 2024-09-12 at 20 47 48" src="https://github.com/user-attachments/assets/fe31780b-ac6b-4a14-89f6-ebe1881116e6">
<br>
<br>

The rule can now be seen in the overview. 

<img width="1792" alt="Screenshot 2024-09-12 at 21 22 56" src="https://github.com/user-attachments/assets/aec6f76d-aaa4-4e6f-a37b-a241df07ab76">
<br>
<br>

The same was done for the Linux machine. These machines were then quite vulnerable and appealing to threat actors. To attract threat actors however, these machines need to also be easy to discover (Linux doesn't have this "problem"): when pinging the Windows machine from my device:

<img width="447" alt="Screenshot 2024-09-12 at 22 21 32" src="https://github.com/user-attachments/assets/93fc4ab9-235a-4bc2-bbd7-166ef5796454">
<br>
<br>

To gain a response back, I turned off the Windows firewall for the domain, public and private profiles. The pings returned a response from the Windows machine after that was done. In the case of Linux, pinging should return a response without any changes. To make sure of this, I pinged the machine and used SSH to log into it, both of which were successful. 

## Downloading SQL Server, SSMS, and setting up SQL Server logs in Event Viewer

Next I downloaded SQL Server to increase the attack surface, along with SQL Server Management Studio (SSMS) which lets you login into SQL Server. Whilst SSMS is not needed and one can use other methods to connect, SSMS has a nice UI unlike SSH for example. Once downloaded, I enabled logging for SQL Server to be ported into Windows Event Viewer with the help of this <a href="https://learn.microsoft.com/en-us/sql/relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log?view=sql-server-ver16"> documentation</a>. Once connected, I made sure to enable logs of both successful and failed login attempts, as the default is failed attempts. To test this, I purposefully failed a login into SQL Server from SSMS and then made a successful login, and checked Event Viewer, selecting "Windows Logs" followed by "Application". A log of the failed attempt was clearly displayed:

<img width="1304" alt="Screenshot 2024-09-12 at 23 00 31" src="https://github.com/user-attachments/assets/1a886040-d959-44ae-9697-306eafbe1c40">
<br>
<br>

Followed by a log of the successful login afterwards:

<img width="1299" alt="Screenshot 2024-09-12 at 23 03 19" src="https://github.com/user-attachments/assets/eff3ffb6-ae54-41f7-8c72-9148edb03632">
<br>
<br>

## Testing Logs with Attach VM

To test I setup another Windows VM (attack-vm) in Central India, to simulate an attack coming from other regions. This was still being done in Azure, under a different resource group and virtual network. Once created, I logged into Remote Desktop Connection and attempted to log into windows-vm 3 times, using the username "Mr. Connect" and some random passwords. Needless to say, this failed. I then installed SSMS on attack-vm and attempted loging into SQL Server using the same username and, once again, random passwords 3 times. I then correctly login as well, to make sure that I could as well as to test again that the logs would reflect this as well. Before doing so however, I failed mutiple logins as I forgot to tick "Trust server certificate" under "Connection Security", creating a good few more logs. Not forgetting linux-vm, I attempted to SSH using the username "mrconnect". 

Checking through both Windows and SQL Server logs I did find my failed attempts, though to my surprise, despite having the machines running for maybe 2 hours, there were already hundreds of login attempts from different users under various usernames. Attracting attention was certainly a success. 

<img width="1296" alt="Screenshot 2024-09-13 at 17 06 13" src="https://github.com/user-attachments/assets/005b087a-98ae-43e2-94b8-a6c8d58eb8e3">
<br>
<br>

The Linux machine logs were also eventful, but I could easily find my attempts while only filtering for "password":

<img width="1168" alt="Screenshot 2024-09-13 at 17 26 22" src="https://github.com/user-attachments/assets/ebd8a241-3590-4386-b6a0-96976419a920">
<br>
<br>

## Setting up SIEM (Microsoft Sentinel)

The next step in this project was to setup a SIEM, however first I needed to have the logs congregate in the same place, a central log repository. On Azure this can be achieved through a Log Analytics Workspace. Once the Log Analytics Workspace had been made, I created Sentinel and added the workspace to it, thereby connecting the two. I then created a watchlist named "geoip", using a csv file with IP range to location matches, to make it possible for Sentinel to show where connections were coming from. 

<img width="1792" alt="Screenshot 2024-09-13 at 19 39 27" src="https://github.com/user-attachments/assets/19dded34-a50f-49e4-bd23-4b0992e1e552">
<br>
<br>

I then enabled Microsoft Defender for Cloud for servers and SQL servers on machines, then changing the subscription settings to have continuous export of all data types to the log analytics workspace. This was done because Microsoft Defender will allow for log forwarding from the virtual machines into the log analytics workspace. 

<img width="1792" alt="Screenshot 2024-09-13 at 20 53 54" src="https://github.com/user-attachments/assets/9838a94d-099c-4db3-801e-3e5b7521c407">
<br>
<br>
























































