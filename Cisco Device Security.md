# Cisco Device Security

## Objective
- Configure a router or the topology provided withthe needed device security (Line Level, Username and Password, Configuring SSH, Login Banners, and NTP)

---



### Before Configuration / Troubleshooting
- No configurations done yet, start with enabling password but will later on use "secret" for better protection.
<img width="1919" height="1079" alt="Cisco Device Security before" src="https://github.com/user-attachments/assets/4d85e5fd-57d6-4649-8016-325df3841e92" />


### During Configuration / Troubleshooting
- Configuring SSH using a username and a password
<img width="1919" height="1079" alt="Cisco Device Security during" src="https://github.com/user-attachments/assets/b3879582-55f8-456b-b92c-087e4d1b3fa8" />


### After Configuration / Troubleshooting
- Configured all the needed device security (console, vty, login banners, ntp) and lastly managing the Switch on the otherside to allow traffic from vlan1
- kapagod haha
<img width="1919" height="1079" alt="Cisco Device Security after" src="https://github.com/user-attachments/assets/18686cbe-5b94-4519-ba0f-ba0e940bf101" />

---


## Configuration Summary

```bash
#Enabling Password and Secret
enable password Flackbox2
enable secret Flackbox1

#Encrypting Passwords
service password-encryption

#Exec timeout on VTY and Console
line console 0 
exec-timeout 15
line vty 0 15
exec-timeout 15

#Access List and setting Banner Login
access-list 1 permit host 10.0.0.10 
line vty 0 15
password Flackbox3
access-class 1 in
login

banner login "
Authorised users only"

#Username and Password
username admin secret Flackbox4
line vty 0 15
login local (important)

#Configuring SSH
ip domain-name flackbox.com
crypto key generate rsa
768
ssh -l admin 10.0.0.1

#Disabling Telnet
line vty 0 15
transport input ssh 
ip ssh version 2

#Enabling access through console
line console 0
login
password Flackbox5

#NTP Network Time Protocol
clock timezone PST -8
ntp server 10.0.1.100
sh clock
sh ntp status

#Switch Management
int vlan 1
ip add 10.0.1.50 255.255.255.0
no shut
exit
default-gateway 10.0.1.1

#NOTES
ssh = Flackbox4
telnet = Flackbox3
enable = Flackbox1
console = Flackbox5

