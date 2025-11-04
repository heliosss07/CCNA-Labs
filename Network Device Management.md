# Network Device Management

## Objective
- Configure a Syslog and SNMP logging

---



### Before Configuration / Troubleshooting
- SNMP hasn't been configured yet and loggings haven't been configured to send to the external syslog server at 100.0.0.100
<img width="1919" height="1079" alt="Network Device Management before" src="https://github.com/user-attachments/assets/8a2aae02-1511-4981-8a2e-833ac56f8a4e" />

### During Configuration / Troubleshooting
- Configured SNMP and Syslog Verifying the levels
<img width="1919" height="1079" alt="Network Device Management during" src="https://github.com/user-attachments/assets/bcc900ea-0253-4993-a176-24a92f1ab044" />

### After Configuration / Troubleshooting
- SNMP and SysLog is configured and verified using the external server on 10.0.0.100
<img width="1919" height="1079" alt="Network Device Management after" src="https://github.com/user-attachments/assets/a81b1c53-c22c-4671-bed0-96c0b896eec5" />

---


## Configuration Summary

```bash
#CONFIGURING SNMP COMMUNITIES
snmp-server community Flackbox1 ro
snmp-server community Flackbox2 rw

#CONFIGURING R1 TO EXTERNAL SYSLOG SERVER WITH SEVERITY LEVELS
logging 10.0.0.100
logging trap debugging (0-7)

#Verifying Levels
sh logging

#CHECK EXTERNAL SYSLOG SERVER
Services -> SYSLOG


