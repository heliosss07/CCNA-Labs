# Open Shortest Path First (OSPF)

## Objective
- Configure the topology to have OSPF, First within a Single Area and Multi-Area as well as demo of how DR and BDR are assigned. kakastress tong part nato haha.

---



### Before Configuration / Troubleshooting
- No configurations done yet, only the loopback interfaces has been configured.
<img width="1919" height="1079" alt="OSPF before" src="https://github.com/user-attachments/assets/275bc4a9-eef0-4ded-a09f-cab0dfb320cd" />

### During Configuration / Troubleshooting
- Single Area OSPF has been configured and bandwidth and cost has been changed to provide load balancing
<img width="1919" height="1079" alt="OSPF during" src="https://github.com/user-attachments/assets/8e100935-c967-4bdf-aad2-cf87533ca5b5" />
- Configured Multi-Area OSPF and seting up the ABRs (R2 & R5) to network 10.0.0.0/24 on area 1 and 10.1.0.0/24 on area 0.
<img width="1919" height="1079" alt="OSPF during2multi area" src="https://github.com/user-attachments/assets/ddd6732f-89b1-4eee-a81a-8966cb64ced3" />

### After Configuration / Troubleshooting
- Finished all the needed configurations and showed how DRs and BDRs are assigned based on their router ID.
<img width="1919" height="1079" alt="OSPF after" src="https://github.com/user-attachments/assets/13eb002e-1749-47cb-9e27-f4fa36ddfc2a" />


---


## Configuration Summary

```bash
#ENABLING LOOPBACK INTERFACES (R1-R5)
en 
conf t
int loopback0
ip add 192.168.0.1 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.2 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.3 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.4 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.5 255.255.255.255

#SINGLE AREA OSPF(R1-R5)
router ospf 1
network 10.0.0.0 0.255.255.255 area 0
network 192.168.0.0 0.0.0.255 area 0

#REFERENCE BANDWIDTH
router ospf 1
auto-cost reference-bandwidth 100000

#OSPF COST FOR LOADBALANCING
R1
int f1/1
ip ospf cost 1500

R5
int f0/1
ip ospf cost 1500
int f0/0
ip ospf cost 1500

R4
int f1/0
ip ospf cost 1500

#PASSIVE INTERFACE(on R4 since it's the router closest on the "internet" router
router ospf 1
passive-interface f1/1
network 203.0.113.0 0.0.0.255 area 0

#DEFAULT STATIC ROUTE
ip route 0.0.0.0 0.0.0.0 203.0.113.2

router ospf 1
default-information originate

#MULTI-AREA OSPF(R3-4 (0), R1(1), R2-5(ABRs))
sh run | section ospf

R1
router ospf 1
network 10.0.0.0 0.255.255.255 area 1
network 192.168.0.0 0.0.0.255 area 1
end
copy run start
reload

R2
router ospf 1
no network 10.0.0.0 0.255.255.255 area 0
network 10.1.0.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.255 area 1
end
copy run start
reload

R5
router ospf 1
no network 10.0.0.0 0.255.255.255 area 0
network 10.1.3.0 0.0.0.255 area 0
network 10.0.3.0 0.0.0.255 area 1
end
copy run start
reload


#SUMMARIZING ROUTES ON ABRs
R2
router ospf 1
area 0 range 10.1.0.0 255.255.0.0
area 1 range 10.0.0.0 255.255.0.0

R5
router ospf 1
area 0 range 10.1.0.0 255.255.0.0
area 1 range 10.0.0.0 255.255.0.0

sh ip ospf database

#DR and BDR Designated Routes
en 
conf t
int loopback0
ip add 192.168.0.6 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.7 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.8 255.255.255.255

en 
conf t
int loopback0
ip add 192.168.0.9 255.255.255.255

#ENABLE OSPF AREA 0 on loopback and f0/0
router ospf 1
network 172.16.0.0 0.0.0.255 area 0
network 192.168.0.0 0.0.0.255 area 0

#BANDWIDTH
router ospf 1
auto-cost reference-bandwidth 100000

#R6 AS DR
int f0/0 
ip ospf priority 100
end

sh ip ospf int f0/0
