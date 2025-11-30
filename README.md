# Sudden-Network-Slowdown
The server team has noticed a significant network performance degradation on some of their older devices attached to the network in the 10.0.0.0/16 network. After ruling out external DDoS attacks, the security team suspects something might be going on internally.

Timeline Summary and Findings:

Seanji-slowdown was found failing several connection request against itself and another host on the same network:
<img width="609" height="78" alt="image" src="https://github.com/user-attachments/assets/dbfa7253-428d-43e3-ba08-3b80d23ff801" />
<img width="912" height="130" alt="image" src="https://github.com/user-attachments/assets/4189f695-ea8d-4045-949e-5948cdb9a2eb" />

—-----------

After observing a failed connection request from a suspected host (10.1.0.176) in chronological order, I noticed a port scan was taking place due to the sequential order of the ports. There were several port scans being conducted:
<img width="322" height="101" alt="image" src="https://github.com/user-attachments/assets/e017c5a3-4d8a-4934-b5d9-431842e44c3b" />
<img width="929" height="196" alt="image" src="https://github.com/user-attachments/assets/d6dff87a-73f9-4c79-90b7-8576759fc198" />
<img width="925" height="223" alt="image" src="https://github.com/user-attachments/assets/8381b78e-d6b7-4231-b38b-4448a28492b7" />

—-----------

We pivoted to the DeviceProcessEvents table to see if we could see anything that was suspicious around the time the ports scan started. We noticed a PowerShell script launching at poertscan.ps1 launch at 2025-11-26T22:20:24.0440687Z:
<img width="610" height="141" alt="image" src="https://github.com/user-attachments/assets/d4d48ef4-db58-4bdd-82ec-7bd7f99b4bda" />
<img width="928" height="32" alt="image" src="https://github.com/user-attachments/assets/660fc137-8a4a-4d7b-952c-2fd26bb0b1d2" />

—-----------

I logged into the suspected computer and observed the powershell script that was used to conduct the port scan:
<img width="936" height="242" alt="image" src="https://github.com/user-attachments/assets/6381a59e-46dc-44c6-9e3a-9d68bbd6033f" />

—-----------

We observed the port scan script was launched by the seanji account, this is not expected behavior and is not something that was setup by the admins, so I isolated the device and ran a malware scan.

—-----------

The malware scan produced no results, so out of caution, we kept the device isolated and put in a ticket to have it reimaged/rebuilt 

—-----------

MITRE ATT&CK Framework Related TTPs:
## TA0043 – Reconnaissance
- **T1046 – Network Service Scanning**

## TA0002 – Execution
- **T1059.001 – PowerShell**

## TA0004 – Privilege Escalation
- **T1078.003 – Valid Accounts: Local Accounts**

## TA0007 – Discovery
- **T1049 – System Network Connections Discovery**

## TA0008 – Lateral Movement
- **T1021 – Remote Services**



