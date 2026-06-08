## OSPF TROUBLESHOOTING LAB

### Lab:
- OSPF-Troubleshooting-Lab

### Objective:
- Troubleshoot an OSPF routing issue caused by an area mismatch between R2-CORE and R3-BRANCH.

### Goal:
- Restore connectivity between PC1 and PC3.

------------------------------------
### Topology
- PC1 --- SW1 --- R1-HQ --- R2-CORE --- R3-BRANCH --- SW3 --- PC3

------------------------------------
### IP Addressing
PC1:
- IP Address: 192.168.10.10/24
- Default Gateway: 192.168.10.1

R1-HQ:
- G0/0: 192.168.10.1/24
- G0/1: 10.0.12.1/30

R2-CORE:
- G0/0: 10.0.12.2/30
- G0/1: 10.0.23.2/30

R3-BRANCH:
- G0/0: 10.0.23.1/30
- G0/1: 192.168.30.1/24

PC3:
- IP Address: 192.168.30.10/24
- Default Gateway: 192.168.30.1

------------------------------------
### Problem
- PC1 cannot ping PC3.
- OSPF routes are missing.
- R2-CORE and R3-BRANCH are not forming an OSPF neighbor relationship.

### Root Cause
- OSPF area mismatch between R2-CORE and R3-BRANCH.
- R2-CORE uses area 0.
- R3-BRANCH incorrectly uses area 1.
- Both routers must use the same OSPF area on the shared 10.0.23.0/30 link.

Broken Line
- network 10.0.23.0 0.0.0.3 area 1

Result:
- OSPF neighbor does not form.

------------------------------------
### Fix
- Correct the OSPF area on R3-BRANCH.

router ospf 1
 - no network 10.0.23.0 0.0.0.3 area 1
 - network 10.0.23.0 0.0.0.3 area 0

------------------------------------
### Verification Commands
- show ip interface brief
- show ip ospf neighbor
- show ip route ospf
- show running-config | section router ospf
- ping 10.0.23.2
- ping 192.168.30.10
- traceroute 192.168.30.10

------------------------------------
### Expected Result
- Interfaces are up/up.
- R3-BRANCH can ping R2-CORE.
- OSPF neighbor state becomes FULL.
- Remote OSPF routes appear.
- PC1 can ping PC3.
- PC3 can ping PC1.

------------------------------------
### Skills Demonstrated
- OSPF troubleshooting
- Neighbor adjacency verification
- Routing table analysis
- Area mismatch detection
- Root-cause analysis
- End-to-end connectivity testing
- Cisco IOS documentation

### Summary
- This lab simulates a realistic routing issue where branch connectivity fails because of an OSPF area mismatch.
- After correcting the OSPF area on R3-BRANCH, the OSPF neighbor relationship becomes FULL, routes are restored, and end-to-end connectivity works successfully.
