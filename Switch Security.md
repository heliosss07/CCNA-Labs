# Switch Security

## Objective
COnfigure specific ports to the allowed number of host and configure sticky or manually configure the MAC-Address that is dedicated for that port

---


### Before Configuration / Troubleshooting
Unconfigured f0/1 and f0/2.
<img width="948" height="939" alt="Port Security before" src="https://github.com/user-attachments/assets/3c1eea51-0a85-4d3c-9ebc-9c0afca1093e" />



### During Configuration / Troubleshooting
disabled unused ports and configuring port-security
<img width="1919" height="1079" alt="Port Security during" src="https://github.com/user-attachments/assets/3795b75e-18c8-4813-a49f-68e0cdd93a91" />



### After Configuration / Troubleshooting
- int f0/1 has 2 max host and 1 static MAC-Address 
- int f0/2 has 1 max host and 1 mac address learned through sticky
<img width="955" height="721" alt="Port Security after" src="https://github.com/user-attachments/assets/3d6c855b-0053-41c0-b44e-5f283e4c920a" />



---

## Configuration Summary

```bash
#DISABLING UNUSED PORTS
sh ip int br
int range f0/3 - 24
shut
int range g0/1 - 2
shut

#f0/1 port security
int f0/1
switch mode access
switchport port-security maximum 2
switchport port-security mac-address 0000.1111.1111

#fo/2 port security
int f0/2
switch mode access
switchport port-security
switchport port-security mac-address sticky

#Verifying
sh mac-address table
sh port-security int f0/1
sh port-security int f0/2
sh port-security 
