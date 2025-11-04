# Spanning Tree Protocol (STP) Lab

## Objective
To analyze and troubleshoot network loops using the Spanning Tree Protocol (STP) in a switched network environment.Pogi pogi



## Network Topology

### Before Troubleshooting
The initial configuration contained redundant links between switches, resulting in a broadcast storm and unstable network behavior.

<img width="1919" height="1079" alt="stp before" src="https://github.com/user-attachments/assets/61ddca84-ba62-4a9b-be72-a74c6f628975" />

### During Troubleshooting
<img width="1919" height="1079" alt="stp during" src="https://github.com/user-attachments/assets/6cb11e2f-3a49-4576-a638-aebceb6ca722" />

### After Troubleshooting
After implementing STP, redundant paths were automatically disabled, and a stable topology was established with a designated root bridge.

<img width="1919" height="1079" alt="stp after" src="https://github.com/user-attachments/assets/82352389-86ed-4545-a990-b2dd0696fd78" />

---

## Configuration Summary


The following Cisco IOS commands were applied during the lab:

```bash
#For configuring the switch RPVST and making sure CD1 is the root bridge.POGI
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root primary
show spanning-tree

#Configure on both switch portfast and bpduguard to prevent any rogue switch from connecting. Mga Hacker
int f0/1
spanning-tree portfast
spanning-tree bpduguard enable

#Protect the root bridges (CD! and CD2) using rootguard on all ports below them
int f0/21 inf f0/24
spanning-tree guard root


