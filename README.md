# Implementing a SOC and Honeynet in Azure

## Objective

## Setting up VMs on Azure

I created two Virtual Machines (VMs), Windows and Linux, making sure that they are in the same region to allow them to be part of the same virtual network. Both machines boast 2 CPUs and some of the lowest prices I could find.
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



































































