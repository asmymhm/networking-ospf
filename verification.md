# Project B - Verification

This document verifies the correct configuration and functionality of **Project B** network topology, including routers, PCs, and OSPF routing.

---

## 1. Network Topology

- **Routers:** R1, R2, R3  
- **PCs:** PC1, PC2, PC3  
- **Connections:**  
  - R1 ↔ R2  
  - R2 ↔ R3  
  - R1 ↔ PC1  
  - R2 ↔ PC2  
  - R3 ↔ PC3  
- **Routing Protocol:** OSPF

---

## 2. Router Configuration Verification

### 2.1 R1
show running-config

show ip route
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C       10.0.12.0/24 is directly connected, GigabitEthernet0/1
L       10.0.12.1/32 is directly connected, GigabitEthernet0/1
O       10.0.23.0/24 [110/2] via 10.0.12.2, 01:18:54, GigabitEthernet0/1
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0
O    192.168.3.0/24 [110/3] via 10.0.12.2, 01:18:54, GigabitEthernet0/1

show ip ospf neighbor
R1#show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/  -        00:00:34    10.0.12.2       GigabitEthernet0/1

### 2.2 R2

show running-config

show ip route

show ip ospf neighborR2#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 4 subnets, 2 masks
C       10.0.12.0/24 is directly connected, GigabitEthernet0/0
L       10.0.12.2/32 is directly connected, GigabitEthernet0/0
C       10.0.23.0/24 is directly connected, GigabitEthernet0/1
L       10.0.23.2/32 is directly connected, GigabitEthernet0/1
O    192.168.1.0/24 [110/2] via 10.0.12.1, 01:25:14, GigabitEthernet0/0
O    192.168.3.0/24 [110/2] via 10.0.23.3, 00:46:14, GigabitEthernet0/1

show ip ospf neighbor
R2#show ip ospf neighbor 

Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           1   FULL/DR         00:00:37    10.0.23.3       GigabitEthernet0/1
1.1.1.1           0   FULL/  -        00:00:30    10.0.12.1       GigabitEthernet0/0

### 2.3 R3

show running-config


show ip route
R3#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
O       10.0.12.0/24 [110/2] via 10.0.23.2, 01:26:40, GigabitEthernet0/0
C       10.0.23.0/24 is directly connected, GigabitEthernet0/0
L       10.0.23.3/32 is directly connected, GigabitEthernet0/0
O    192.168.1.0/24 [110/3] via 10.0.23.2, 01:25:30, GigabitEthernet0/0
     192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.3.0/24 is directly connected, GigabitEthernet0/1
L       192.168.3.1/32 is directly connected, GigabitEthernet0/1


show ip ospf neighbor
R3#show ip ospf neighbor 

Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:33    10.0.23.2       GigabitEthernet0/0


###  3. PC Configuration Verification


| PC  | IP Address   | Default Gateway | Test Command | Expected Result |
| --- | ------------ | --------------- | ------------ | --------------- |
| PC1 | 192.168.1.10 | 192.168.1.1     | ping PC2     | Success         |
| PC2 | 192.168.3.10 | 192.168.3.1     | ping PC1     | Success         |



