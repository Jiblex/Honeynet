# Implementing a SOC and Honeynet in Azure

## Azure 

2 Vms created, in same region and same virtual network. Now must open them up so that they accept all traffic coming from the Internet. This can be done in the network security groups of the two machines by creating a rule that allows all traffic. Currently if traffic inbound from the Internet is not RDP, then it is evaluated by the next set of rules which can be seen until it is eventually denied if it doesn't pass them. 

<img width="1792" alt="Screenshot 2024-09-12 at 20 40 57" src="https://github.com/user-attachments/assets/a47e8a23-36df-47c0-af05-a0af4ed15d52">
<br>
<br>

To let all traffic in I deleted the RDP rule, and created the new rule:

<img width="1792" alt="Screenshot 2024-09-12 at 20 47 48" src="https://github.com/user-attachments/assets/fe31780b-ac6b-4a14-89f6-ebe1881116e6">
<br>
<br>




































































