# Network Address translation (NAT)

## Objective
- Configure a NAT on the router to translate private IPs to Public IPs. In this lab gagawa ng Static & Dynamic NAT, and PAT focus on PAT since eto ginagamit sa real world, which uses
one public IP for multiple private IPs with different ports meanwhile, static is used for servers. Dynamic is less used. Hatdog

---



### Before Configuration / Troubleshooting
- No configuratiions done yet.
<img width="1919" height="1079" alt="NAT before" src="https://github.com/user-attachments/assets/a9eef766-ea0a-48e5-acb4-9f3d580b9637" />

### During Configuration / Troubleshooting
- Removed the previous configurations
- Configured PAT using DHCP
<img width="1919" height="1079" alt="NAT during" src="https://github.com/user-attachments/assets/86b480af-6ab5-4c1b-b31e-306cd5b10244" />


### After Configuration / Troubleshooting
- All configurations done and one public ip is being used for multiple host with different port numbers. woww magic
<img width="958" height="605" alt="NAT after" src="https://github.com/user-attachments/assets/36556493-7787-445c-8db8-becfa508d227" />

---


## Configuration Summary

```bash
Network Address Translation (NAT) and PAT

#STATIC NAT
R1(config)#int f0/0
R1(config-if)#ip nat outside
R1(config-if)#int f0/1
R1(config-if)#ip nat inside
R1(config-if)#exit

R1(config)#ip nat inside source static 10.0.1.10 203.0.113.3

#DYNAMIC NAT
R1(config)#int f0/0
R1(config-if)#ip nat outside
R1(config-if)#int f1/0
R1(config-if)#ip nat inside
R1(config-if)#exit

R1(config)#ip nat pool Flackbox 203.0.113.4 203.0.113.12 netmask 255.255.255.240
R1(config)#access-list 1 permit 10.0.2.0 0.0.0.255
R1(config)#ip nat inside source list 1 pool Flackbox 

# DEBUGGING (used a PAT but dynamic for using the last available host for multiple ports. Alam mo na yan)
R1#debug ip nat
R1#clear ip nat translation *

R1(config)#ip nat inside source list 1 pool Flackbox overload

#Cleanup (removing all configurations) 
R1(config)#int f0/0
R1(config-if)#no ip nat outside
R1(config-if)#int f0/1
R1(config-if)#no ip nat inside
R1(config-if)#int f1/0
R1(config-if)#no ip nat inside
R1(config-if)#end
R1#clear ip nat translation *
R1#conf t
R1(config)#no ip nat inside source list 1 pool Flackbox overload
R1(config)#no ip nat pool Flackbox 203.0.113.4 203.0.113.12 netmask 255.255.255.240
R1(config)#no access-list 1 

R1#sh run | section nat
R1#sh run | section access

#DHCP/PAT for a single public address
R1(config-if)#int f0/0
R1(config-if)#shut
R1(config-if)#no ip address 
R1(config-if)#ip address dhcp 
R1(config-if)#no shut

R1(config)#int f0/0
R1(config-if)#ip nat outside
R1(config-if)#int f1/0
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#access-list 1 permit 10.0.2.0 0.0.0.255
R1(config)#ip nat inside source list 1 interface f0/0 overload

