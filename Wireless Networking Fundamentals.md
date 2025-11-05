# Wireless Networking Fundamentals

## Objective
- Configure a Corporate and Guest WLAN 

---


### Before Configuration / Troubleshooting
- No configurations done yet on the WLC and Multilayer switch.
<img width="957" height="681" alt="Wireless Fundamentals before" src="https://github.com/user-attachments/assets/b735fa14-f471-4e8d-9627-e8dfb7d52de0" />



### During Configuration / Troubleshooting
- Configuring the WLC to create DHCP on Corporate and Guest.
<img width="1919" height="1079" alt="Wireless Fundamentals during" src="https://github.com/user-attachments/assets/a96454db-2a11-4f0f-b3f7-517205d0a316" />

### After Configuration / Troubleshooting
- Verifying the connectivity between clients (guest adn corporate)
<img width="1919" height="1079" alt="Wireless Fundamentals after" src="https://github.com/user-attachments/assets/727bd17b-1e37-4d24-8566-3f34e1741ff9" />


---



## Configuration Summary

```bash
#VLAN 10 MANAGEMENT
vlan 10
name Management
int vlan 10
ip address 192.168.10.1 255.255.255.0

#DNS 
cisco-capwap-controller
192.168.10.11

#VLAN 22 Corporate
vlan 22
name Corporate
int vlan 22
ip address 192.168.22.1 255.255.255.0

#VLAN 23 Guest
vlan 23
name Guest
int vlan 23
ip address 192.168.23.1 255.255.255.0

#G1/0/5 FOR WLC
int g1/0/5
switchport trunk encapsulation dot1q
switchport mode trunk allowed vlan 10,22,23
spanning-tree portfast

#G1/0/3-4 FOR LIGHTWEIGHT ACCESS POINTS
int range g1/0/3 - 4
switchport mode access
switchport access vlan 10
spanning-tree portfast

#DHCP and WLC FOR GUEST AND CORPORATE
Pool start address 
192.168.23.101 - 254
192.168.22.101 - 254

#LOGICAL INTERFACES
22
192.168.22.11 (ip add)
255.255.255.0 (netmask)
192.168.22.1 (default-router)
192.168.10.11 (DHCP/WLAN)

#WLANs
Corporate = WPA2 and AAA
Guest = WPA2-PSK

#JOINING OF CLIENTS
id:Flackbox
password:Flackbox 3 
(corporate)

PSK = Flackbox 3

#Note: don't forget to change the SSID to Corporate or Guests

#VERIFY CONNECTIVITY BETWEEN CLIENTS
ping 192.168.22.101
ping 
