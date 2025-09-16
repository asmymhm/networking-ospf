# Project B - OSPF Topology

This repository contains the configuration and verification for **OSPF routing** across multiple routers in a small network topology.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
---

## Table of Contents

1.  [Project Overview]
2.  [Lab Topology]
3.  [IP Addressing Table]
4.  [Lab Steps]
5.  [Router Configuration]
6.  [PC Configuration]
7.  [Verification]
8.  [End-to-End Connectivity]
9.  [Folder Structure]
10. [License]


---

## Project Overview

- **Objective:** Implement OSPF routing to enable full connectivity between PCs.
- **Technologies Used:** Cisco Packet Tracer / GNS3, OSPF routing.
- **Routers:** R1, R2, R3
- **PCs:** PC1, PC2

---
## Lab Topology

<img src="topology-diagram\topology.png" alt="OSPF TOPOLOGY" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">


        PC1 (192.168.1.10)
                |
              R1
         G0/0 | G0/1
              R2
         G0/0 | G0/1
              R3
                |
        PC2 (192.168.3.10)

R1 ↔ R2 → 10.0.12.0/24

R2 ↔ R3 → 10.0.23.0/24

LANs: 192.168.1.0/24 (PC1), 192.168.3.0/24 (PC2)

---

## IP Addressing Table

| Device | Interface | IP Address   | Subnet Mask   |
| ------ | --------- | ------------ | ------------- |
| R1     | G0/0      | 192.168.1.1  | 255.255.255.0 |
| R1     | G0/1      | 10.0.12.1    | 255.255.255.0 |
| R2     | G0/0      | 10.0.12.2    | 255.255.255.0 |
| R2     | G0/1      | 10.0.23.2    | 255.255.255.0 |
| R3     | G0/0      | 10.0.23.3    | 255.255.255.0 |
| R3     | G0/1      | 192.168.3.1  | 255.255.255.0 |
| PC1    | NIC       | 192.168.1.10 | 255.255.255.0 |
| PC2    | NIC       | 192.168.3.10 | 255.255.255.0 |

Default gateways: PC1 → 192.168.1.1, PC2 → 192.168.3.1

---

## Lab Steps

Step 1: Configure Router Interfaces
```text
interface G0/0
ip address <IP> <Subnet Mask>
no shutdown
```

```text
interface G0/1
ip address <IP> <Subnet Mask>
no shutdown
```

Step 2: Enable OSPF and Assign Router IDs
```text
router ospf 1
router-id <Router-ID>
log-adjacency-changes
network <Network> <Wildcard> area 0
```
Apply for R1, R2, R3 with respective router IDs and networks.

Step 3: Verify OSPF Neighbor Adjacency
```text
show ip ospf neighbor
```
Step 4: Verify OSPF Routing Table
```text
show ip route ospf
```
All remote LANs should be learned via OSPF.

Step 5: Configure PC IPs and Default Gateways

Ensure PCs can ping their default gateway.

Test end-to-end ping between LANs: PC1 → PC2.

---

## Router Configuration

R1 configs/router-configs/R1-config.txt

```text
hostname R1
interface G0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface G0/1
 ip address 10.0.12.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 1.1.1.1
 log-adjacency-changes
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.255 area 0
 ```

 R2 configs/router-configs/R2-config.txt

 ```text
 hostname R2
interface G0/0
 ip address 10.0.12.2 255.255.255.0
 no shutdown
interface G0/1
 ip address 10.0.23.2 255.255.255.0
 no shutdown
router ospf 1
 router-id 2.2.2.2
 log-adjacency-changes
 network 10.0.12.0 0.0.0.255 area 0
 network 10.0.23.0 0.0.0.255 area 0
```

R3 configs/router-configs/R3-config.txt

```text
hostname R3
interface G0/0
 ip address 10.0.23.3 255.255.255.0
 no shutdown
interface G0/1
 ip address 192.168.3.1 255.255.255.0
 no shutdown
router ospf 1
 router-id 3.3.3.3
 log-adjacency-changes
 network 10.0.23.0 0.0.0.255 area 0
 network 192.168.3.0 0.0.0.255 area 0
```

---

## PC Configuration

PC1 configs/pc-configs/PC1-config.txt

IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1

PC2 configs/pc-configs/PC2-config.txt

IP Address: 192.168.3.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.3.1

---

## Verification

R1 Neighbors

```text
R1#show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/  -        00:00:34    10.0.12.2       GigabitEthernet0/1
```
<img src="screenshots\show ip ospf neighbor R1.png" alt="OSPF R1" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

R2 Neighbors

```text
R2#show ip ospf neighbor 

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           1   FULL/DR         00:00:37    10.0.23.3       GigabitEthernet0/1
1.1.1.1           0   FULL/  -        00:00:30    10.0.12.1       GigabitEthernet0/0
```
<img src="screenshots\show ip ospf neighbor R2.png" alt="OSPF R2" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

R3 Neighbors

```text
R3#show ip ospf neighbor 

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:33    10.0.23.2       GigabitEthernet0/0
```
<img src="screenshots\show ip ospf neighbor R3.png" alt="OSPF R3" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

---

Sample OSPF Routing Table (R1)

<img src="screenshots\show ip route ospf R1.png" alt="OSPF TABLE R1" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

Sample OSPF Routing Table (R2)

<img src="screenshots\show ip route ospf R2.png" alt="OSPF TABLE R2" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

Sample OSPF Routing Table (R3)

<img src="screenshots\show ip route ospf R3.png" alt="OSPF TABLE R3" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

---

## End-to-End Connectivity

| Source | Destination | Result  |
| ------ | ----------- | ------- |
| PC1    | PC2         | Success |
| PC3    | PC1         | Success |
| R1     | R3          | Success |
| R3     | R1          | Success |

Ping between PC1 ↔ PC2 is successful

OSPF adjacency established on all routers

Routing tables updated correctly via show ip route ospf

<img src="screenshots\ping pc1 to pc2.png" alt="OSPF TABLE R3" style="border:1px solid #ddd; padding:5px; max-width:100%; height:auto;">

---

## Folder Structure

networking-ospf-lab/
│
├── README.md
├── router-configs/
│   ├── R1-config.txt
│   ├── R2-config.txt
│   └── R3-config.txt
├── pc-configs/
│   ├── PC1-config.txt
│   └── PC2-config.txt
├── screenshots/
│   ├── show ip ospf neighbor R1.png
│   ├── show ip ospf neighbor R2.png
│   ├── show ip ospf neighbor R3.png
│   ├── show ip route ospf R1.png
│   ├── show ip route ospf R2.png
│   ├── show ip route ospf R3.png
│   └── ping pc1 to pc2.png
└── topology-diagram/
    └── topology.png

---

## License

<a href="LICENSE">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT License" />
</a>

This project is licensed under the [MIT License](LICENSE).

---


