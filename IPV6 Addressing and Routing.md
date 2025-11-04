# IPV6 Addressing and Routing

## Objective
- Configure IPV6 Addressing and Routing on all routers and ensure connectivity using a Static Route. sheesh

---

### Before Configuration / Troubleshooting
- IPV4 is configured but IPV6 is still not configured
<img width="1919" height="1079" alt="IPV6 before" src="https://github.com/user-attachments/assets/d4ef7cae-90c1-486b-9b17-db2abb9be8e9" />


### During Configuration / Troubleshooting
- Enabling the unicast routing command (this should be done from the start, but for the sake of the lab demo, it was not configured at the start to show the importance of enabling it first) baka english yan.
- Configuring the static routes for the topology.
<img width="1919" height="1079" alt="IPV6 during" src="https://github.com/user-attachments/assets/2b330545-33b4-45d6-be0a-a38b4087f0fb" />


### After Configuration / Troubleshooting
- Configured all the static routes for IPV6 and verifying the connectivity between PC1 and PC2
<img width="1919" height="1079" alt="IPV6 after" src="https://github.com/user-attachments/assets/751a6626-75f4-45a5-9398-7353bd4f6c70" />

---


## Configuration Summary

```bash
#Verify IPV4 Connectivity
sh ip int br
sh ip route

#R1 Address
int f0/1
ipv6 add 2001:db8::1/64
no shut
int f0/0
ipv6 add 2001:db8:0:1::1/64
no shut

#R2 Address
int f0/1
ipv6 add 2001:db8:0:2::2/64
no shut
int f0/0
ipv6 add 2001:db8:0:1::2/64
no shut

#R3 Address
int f0/1
ipv6 add 2001:db8:0:3::1/64
no shut
int f0/0
ipv6 add 2001:db8:0:2::1/64
no shut

#EUI-64 on PC1 and PC2
int f0/0
ipv6 add 2001:db8::/64 eui-64
no shut

int f0/0
ipv6 add 2001:db8:0:3::/64 eui-64
no shut

#PC1 2001:DB8::200:CFF:FE47:14C0
#PC2 2001:DB8:0:3:201:C7FF:FE50:8E8A

#Link-local Address on R1-R3

int f0/0
ipv6 add FE80::1 link-local
int f0/1
ipv6 add FE80::1 link-local

int f0/0
ipv6 add FE80::2 link-local
int f0/1
ipv6 add FE80::2 link-local

int f0/0
ipv6 add FE80::3 link-local
int f0/1
ipv6 add FE80::3 link-local

#STATIC ROUTING
-default gateway
PC1 ipv6 route ::/0 2001:db8::1
PC2 ipv6 route ::/0 2001:db8:0:3::1

#Enable "ipv6 unicast-routing"
ipv6 unicast-routing

#static route on R1
ipv6 route 2001:db8:0:2::/64 2001:db8:0:1::2
ipv6 route 2001:db8:0:3::/64 2001:db8:0:1::2

#static route on R2
ipv6 route 2001:db8::/64 2001:db8:0:1::1
ipv6 route 2001:db8:0:3::/64 2001:db8:0:2::1

#static route on R3
ipv6 route 2001:db8:0:1::/64 2001:db8:0:2::2
ipv6 route 2001:db8::/64 2001:db8:0:2::2
