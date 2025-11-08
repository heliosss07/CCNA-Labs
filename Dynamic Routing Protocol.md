
## Objective
- Be able to configure the different dynamic routing protocols such as RIP, EIGRP, and OSPF as well as troubleshooting incase a link goes down. englishero amp
<img width="906" height="466" alt="image" src="https://github.com/user-attachments/assets/182ffa5b-9f42-4382-84d4-661de3ceda84" />


---



### Before Configuration / Troubleshooting
- enabling RIPv2 on all the routers
<img width="1919" height="1079" alt="Dynamic Routing before" src="https://github.com/user-attachments/assets/95125b2d-bf25-4323-9412-b0e6cb0c19c7" />


### During Configuration / Troubleshooting
- making static routes for the whole topology incase the link between R2 and R1 shuts down which in this case it will still have a route to go to even though eigrp or any routing protocol is configured.
- set it to an AD of 95 so that EIGRP(90) will still be preffered if it's working.
<img width="1919" height="1079" alt="Dynamic Routing during" src="https://github.com/user-attachments/assets/ab968083-6d73-4c38-a913-456d2310721c" />

### After Configuration / Troubleshooting
- all configurations are done and eigrp is still configured however f1/1 on R1 is set as passive int as well as the loopback add of R5 so no routing packets will be sent through the link of R1 and R5 so all traffic will go to R4 as seen on the routing table
<img width="1919" height="1079" alt="Dynamic Routing after" src="https://github.com/user-attachments/assets/0c883b4e-d6f2-4d86-a909-0b09fce0b22d" />


---


## Configuration Summary

```bash
#RIPv1
router rip 
network 10.0.0.0
no auto-summary
debug ip rip(R1)

#RIPv2
router rip
version 2
undebug all
sh ip rip database


#OSPF
router ospf 1
network 10.0.0.0 0.255.255.255 area 0
sh ip ospf database

no router ospf 1

#EIGRP
router eigrp 100
no auto-summary
network 10.0.0.0 0.255.255.255

no router rip (on R5)
no router eigrp 100(on R5)

#Static Routes (for backup/incase link on R2 goes down and no eigrp)
R1 ip route 10.1.0.0 255.255.0.0 10.0.3.2 95
(AD because we still want EIGRP to be prioritized if its working) 
R2 ip route 10.0.0.0 255.255.0.0 10.1.0.1 95
R3 ip route 10.0.0.0 255.255.0.0 10.1.1.1 95
R4 ip route 10.0.0.0 255.255.0.0 10.1.3.2 95

R5 ip route 10.1.0.0 255.255.0.0 10.1.3.1 95
R5 ip route 10.0.0.0 255.255.0.0 10.0.3.1 95

#Loopback Interfaces
int loopback0
ip add 192.168.0.1 255.255.255.255

int loopback0
ip add 192.168.0.2 255.255.255.255

int loopback0
ip add 192.168.0.3 255.255.255.255

int loopback0
ip add 192.168.0.4 255.255.255.255

int loopback0
ip add 192.168.0.5 255.255.255.255

#INCLUDING LOOPBACK INT ON EIGRP
router eigrp 100
network 192.168.0.0 0.0.0.255

sh ip eigrp neigh

#LOOPBACK AS PASSIVE INT

router eigrp 100
passive-interface loopback0
passive-interface f1/1


