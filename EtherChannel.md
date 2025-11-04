# EtherChannel

## Objective
to configure and troubleshoot EthernetChannel in the topology provided.

---



### Before Configuration / Troubleshooting
Some links are blocked, and half of the bandwidth is not being utilized due to STP. I need to configure EtherChannel on links Acc3 and CD1 and 2, and for Acc3. Layer 3 switches aren't configured yet.

<img width="1919" height="1079" alt="etherchannel before" src="https://github.com/user-attachments/assets/dd1780c7-5722-4be1-a9f5-e8181c02430b" />


### During Configuration / Troubleshooting
setup the EtherChannel needed for both Acc3 and Acc4 connecting to CD1 and CD2

<img width="1918" height="1047" alt="etherchannel during" src="https://github.com/user-attachments/assets/f47349d0-e5b7-4fab-a484-e65e63b717be" />



### After Configuration / Troubleshooting
Finished configuring for layer 2 switches and 3. Slight misunderstanding on why there are blocked ports on layer 3 switches, but OSPF and eth sum results are correct. basta ayaw kona haha.

<img width="669" height="387" alt="etherchannel after1" src="https://github.com/user-attachments/assets/d67d11ad-439c-4565-85fa-913e96e357d4" />

<img width="643" height="553" alt="etherchannel after2" src="https://github.com/user-attachments/assets/bb4898e7-6a87-4c36-b2fa-93e015c8f52d" />

---


## Configuration Summary

```bash
#LACP EtherChannel Configuration
-ACC 3 (LACP)
int range f0/23 - 24
channel-group 1 mode active
int po1
switchport mode trunk
switchport trunk native vlan 199

int range f0/21 - 22
channel-group 2 mode active
int po2
switchport mode trunk
switchport trunk native vlan 199

#PAgP EtherChannel Configuration
-ACC 4 (PAgP)
int range f0/23 - 24
channel-group 1 mode desirable
int po1
switchport mode trunk
switchport trunk native vlan 199

int range f0/21 - 22
channel-group 2 mode desirable
int po2
switchport mode trunk
switchport trunk native vlan 199

#Static EtherChannel Configuration
-CD1 to CD2
int range g0/1 - 2
channel-group 3 mode on
int po3
switchport mode trunk
switchport trunk native vlan 199

#LAYER 3 SWITCH(LACP)
-SW1 to SW2
int range g1/0/1 - 2
no switchport
channel-group 1 mode active
exit
int po1
ip add 192.168.0.1 255.255.255.252
no shut

int range g1/0/1 - 2
no switchport
channel-group 1 mode active
exit
int po1
ip add 192.168.0.2 255.255.255.252
no shut

-SW1 to SW3
int range g1/0/3 - 4
no switchport
channel-group 2 mode active
exit
int po2
ip add 192.168.0.5 255.255.255.252
no shut

int range g1/0/3 - 4
no switchport
channel-group 2 mode active
exit
int po2
ip add 192.168.0.6 255.255.255.252
no shut

-SW2 to SW3
int range g1/0/5 - 6
no switchport
channel-group 3 mode active
exit
int po3
ip add 192.168.0.9 255.255.255.252
no shut

int range g1/0/5 - 6
no switchport
channel-group 3 mode active
exit
int po3
ip add 192.168.0.10 255.255.255.252
no shut

#Configuring OSPF on all Switches
ip routing
router ospf 1 
network 192.168.0.0 0.0.0.255 area 0



