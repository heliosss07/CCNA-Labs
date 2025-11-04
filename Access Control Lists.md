# Access Control Lists (ACLs)

## Objective
-Configure the topology and use Access Control to manage traffic allowed on a specific port/interface using standard and extended access control lists. POGIII

---



### Before Configuration / Troubleshooting
Nothing has been configured yet, all traffic is allowed to connected interfaces via the router.
<img width="1919" height="1079" alt="Access Contorl Lists before" src="https://github.com/user-attachments/assets/fd7342c0-5923-489e-a4d7-6cfd8c20dc5a" />


### During Configuration / Troubleshooting
Denied coming traffic from the subnet 10.0.2.0 /24 and alloqing traffic from the 10.0.1.0/24 subnet  through the int f0/0 on R1. Panis
<img width="1919" height="1079" alt="Access Contorl Lists during" src="https://github.com/user-attachments/assets/c4147c3a-ea30-49d4-bc20-fbda359ffbda" />


### After Configuration / Troubleshooting
Successfully configured the needed configurations (Numbered, Named Extended, Removed Numbered Extended, and verified)
<img width="1919" height="1079" alt="Access Contorl Lists after" src="https://github.com/user-attachments/assets/9d0515bf-1faa-40d6-87e7-455ae9d03d16" />

---


## Configuration Summary

```bash
#Standard Numbered ACLs
R1(config)#access-list 1 deny 10.0.2.0 0.0.0.255
R1(config)#access-list 1 permit 10.0.1.0 0.0.0.255 
R1(config)#int f0/0
R1(config-if)#ip access-group 1 out

#Extended Numbered ACLs
R1(config)#access-list 100 permit tcp host 10.0.1.10 host 10.0.0.2 eq telnet
R1(config)#access-list 100 deny tcp any host 10.0.0.2 eq telnet
R1(config)#access-list 100 permit ip any any
R1(config)#int f1/0
R1(config-if)#ip access-group 100 in

#Remove the extended
R1(config)#int f1/0
R1(config-if)#no ip access-group 100 in

#Named Extended ACLs
R1(config)#ip access-list extended F1/0_in
R1(config-ext-nacl)#permit tcp host 10.0.1.10 host 10.0.0.2 eq tel
R1(config-ext-nacl)#permit tcp host 10.0.1.10 host 10.0.0.2 eq telnet 
R1(config-ext-nacl)#deny tcp any host 10.0.0.2 eq telnet
R1(config-ext-nacl)#permit icmp host 10.0.1.11 host 10.0.0.2 echo
R1(config-ext-nacl)#deny icmp any host 10.0.0.2 echo
R1(config-ext-nacl)#permit ip any any

R1(config-ext-nacl)#int f1/0
R1(config-if)#ip access-group F1/0_in in

