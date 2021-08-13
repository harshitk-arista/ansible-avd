# DC1-BL1B
# Table of Contents
<!-- toc -->

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Name Servers](#name-servers)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SNMP](#snmp)
- [Hardware TCAM Profile](#hardware-tcam-profile)
  - [Hardware TCAM configuration](#hardware-tcam-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Platform](#platform)
  - [Platform Summary](#platform-summary)
  - [Platform Configuration](#platform-configuration)
- [Quality Of Service](#quality-of-service)

<!-- toc -->
# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.200.111/24 | 192.168.200.5 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.200.111/24
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 192.168.200.5 | MGMT |
| 8.8.8.8 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 192.168.200.5
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS |
| ---------- | ---------- |
| default | true |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |


### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| admin | 15 | network-admin |
| cvpadmin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username cvpadmin privilege 15 role network-admin secret sha512 $6$rZKcbIZ7iWGAWTUM$TCgDn1KcavS0s.OV8lacMTUkxTByfzcGlFlYUWroxYuU7M/9bIodhRO7nXGzMweUxvbk8mJmQl8Bh44cRktUj.
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | Ingest gRPC URL | Ingest Authentication Key | Smash Excludes | Ingest Exclude | Ingest VRF |  NTP VRF | AAA Disabled |
| -------------- | --------------- | ------------------------- | -------------- | -------------- | ---------- | -------- | ------ |
| gzip | 192.168.200.11:9910 | telarista | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | MGMT | MGMT | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -ingestgrpcurl=192.168.200.11:9910 -cvcompression=gzip -ingestauth=key,telarista -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -ingestvrf=MGMT -taillogs
   no shutdown
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps |
| ------- | -------- | ---------- |
| example@example.com | DC1_FABRIC DC1-BL1B |  Disabled  |

### SNMP ACLs
| IP | ACL | VRF |
| -- | --- | --- |


### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |

### SNMP VRF Status

| VRF | Status |
| --- | ------ |






### SNMP Device Configuration

```eos
!
snmp-server contact example@example.com
snmp-server location DC1_FABRIC DC1-BL1B
```

# Hardware TCAM Profile

TCAM profile __`vxlan-routing`__ is active

## Hardware TCAM configuration

```eos
!
hardware tcam
   system profile vxlan-routing
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

STP Root Super: **True**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

### Global Spanning-Tree Settings


## Spanning Tree Device Configuration

```eos
!
spanning-tree root super
spanning-tree mode mstp
spanning-tree mst 0 priority 4096
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 150 | Tenant_A_WAN_Zone_1 | - |
| 250 | Tenant_B_WAN_Zone_1 | - |
| 350 | Tenant_C_WAN_Zone_1 | - |

## VLANs Device Configuration

```eos
!
vlan 150
   name Tenant_A_WAN_Zone_1
!
vlan 250
   name Tenant_B_WAN_Zone_1
!
vlan 350
   name Tenant_C_WAN_Zone_1
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet7 | test | routed | - | 10.10.20.20/24 | Tenant_A_WAN_Zone | 9000 | false | - | - |
| Ethernet8 | test | routed | - | 10.10.30.10/24 | Tenant_L3_VRF_Zone | 9000 | false | - | - |
| Ethernet9 | test | routed | - | 10.10.40.20/24 | Tenant_L3_VRF_Zone | 9000 | false | - | - |
| Ethernet10.100 | subinterface test | l3dot1q | - | 10.10.31.10/24 | Tenant_L3_VRF_Zone | 9000 | false | - | - |
| Ethernet10.200 | subinterface test with vlan override | l3dot1q | - | 10.10.41.10/24 | Tenant_L3_VRF_Zone | 9000 | false | - | - |
| Ethernet45 | P2P_LINK_TO_DC1-SPINE1_Ethernet7 | routed | - | 172.31.255.97/31 | default | 1500 | false | - | - |
| Ethernet46 | P2P_LINK_TO_DC1-SPINE2_Ethernet7 | routed | - | 172.31.255.99/31 | default | 1500 | false | - | - |
| Ethernet47 | P2P_LINK_TO_DC1-SPINE3_Ethernet7 | routed | - | 172.31.255.101/31 | default | 1500 | false | - | - |
| Ethernet48 | P2P_LINK_TO_DC1-SPINE4_Ethernet7 | routed | - | 172.31.255.103/31 | default | 1500 | false | - | - |
| Ethernet4000 | My second test | routed | - | 10.1.2.3/12 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet7
   description test
   no shutdown
   mtu 9000
   no switchport
   vrf Tenant_A_WAN_Zone
   ip address 10.10.20.20/24
!
interface Ethernet8
   description test
   no shutdown
   mtu 9000
   no switchport
   vrf Tenant_L3_VRF_Zone
   ip address 10.10.30.10/24
!
interface Ethernet9
   description test
   no shutdown
   mtu 9000
   no switchport
   vrf Tenant_L3_VRF_Zone
   ip address 10.10.40.20/24
!
interface Ethernet10
   no shutdown
   no switchport
!
interface Ethernet10.100
   description subinterface test
   no shutdown
   mtu 9000
   encapsulation dot1q vlan 100
   vrf Tenant_L3_VRF_Zone
   ip address 10.10.31.10/24
!
interface Ethernet10.200
   description subinterface test with vlan override
   no shutdown
   mtu 9000
   encapsulation dot1q vlan 141
   vrf Tenant_L3_VRF_Zone
   ip address 10.10.41.10/24
!
interface Ethernet45
   description P2P_LINK_TO_DC1-SPINE1_Ethernet7
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.97/31
!
interface Ethernet46
   description P2P_LINK_TO_DC1-SPINE2_Ethernet7
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.99/31
!
interface Ethernet47
   description P2P_LINK_TO_DC1-SPINE3_Ethernet7
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.101/31
!
interface Ethernet48
   description P2P_LINK_TO_DC1-SPINE4_Ethernet7
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.103/31
!
interface Ethernet4000
   description My second test
   no shutdown
   mtu 1500
   no switchport
   ip address 10.1.2.3/12
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 192.168.255.15/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 192.168.254.15/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 192.168.255.15/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 192.168.254.15/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan150 |  Tenant_A_WAN_Zone_1  |  Tenant_A_WAN_Zone  |  -  |  false  |
| Vlan250 |  Tenant_B_WAN_Zone_1  |  Tenant_B_WAN_Zone  |  -  |  false  |
| Vlan350 |  Tenant_C_WAN_Zone_1  |  Tenant_C_WAN_Zone  |  -  |  false  |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan150 |  Tenant_A_WAN_Zone  |  -  |  10.1.40.1/24  |  -  |  -  |  -  |  -  |
| Vlan250 |  Tenant_B_WAN_Zone  |  -  |  10.2.50.1/24  |  -  |  -  |  -  |  -  |
| Vlan350 |  Tenant_C_WAN_Zone  |  -  |  10.3.50.1/24  |  -  |  -  |  -  |  -  |


### VLAN Interfaces Device Configuration

```eos
!
interface Vlan150
   description Tenant_A_WAN_Zone_1
   no shutdown
   vrf Tenant_A_WAN_Zone
   ip address virtual 10.1.40.1/24
!
interface Vlan250
   description Tenant_B_WAN_Zone_1
   no shutdown
   vrf Tenant_B_WAN_Zone
   ip address virtual 10.2.50.1/24
!
interface Vlan350
   description Tenant_C_WAN_Zone_1
   no shutdown
   vrf Tenant_C_WAN_Zone
   ip address virtual 10.3.50.1/24
```

## VXLAN Interface

### VXLAN Interface Summary

#### Source Interface: Loopback1

#### UDP port: 4789

#### VLAN to VNI Mappings

| VLAN | VNI |
| ---- | --- |
| 150 | 10150 |
| 250 | 20250 |
| 350 | 30350 |

#### VRF to VNI Mappings

| VLAN | VNI |
| ---- | --- |
| Tenant_A_WAN_Zone | 14 |
| Tenant_B_OP_Zone | 20 |
| Tenant_B_WAN_Zone | 21 |
| Tenant_C_WAN_Zone | 31 |
| Tenant_L3_VRF_Zone | 15 |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 150 vni 10150
   vxlan vlan 250 vni 20250
   vxlan vlan 350 vni 30350
   vxlan vrf Tenant_A_WAN_Zone vni 14
   vxlan vrf Tenant_B_OP_Zone vni 20
   vxlan vrf Tenant_B_WAN_Zone vni 21
   vxlan vrf Tenant_C_WAN_Zone vni 31
   vxlan vrf Tenant_L3_VRF_Zone vni 15
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:dc:00:00:00:0a

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:dc:00:00:00:0a
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true|| MGMT | false |
| Tenant_A_WAN_Zone | true |
| Tenant_B_OP_Zone | true |
| Tenant_B_WAN_Zone | true |
| Tenant_C_WAN_Zone | true |
| Tenant_L3_VRF_Zone | true |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf Tenant_A_WAN_Zone
ip routing vrf Tenant_B_OP_Zone
ip routing vrf Tenant_B_WAN_Zone
ip routing vrf Tenant_C_WAN_Zone
ip routing vrf Tenant_L3_VRF_Zone
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false || MGMT | false |
| Tenant_A_WAN_Zone | false |
| Tenant_B_OP_Zone | false |
| Tenant_B_WAN_Zone | false |
| Tenant_C_WAN_Zone | false |
| Tenant_L3_VRF_Zone | false |


## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT  | 0.0.0.0/0 |  192.168.200.5  |  -  |  1  |  -  |  -  |  - |
| Tenant_A_WAN_Zone  | 10.3.4.0/24 |  1.2.3.4  |  -  |  1  |  -  |  -  |  - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.200.5
ip route vrf Tenant_A_WAN_Zone 10.3.4.0/24 1.2.3.4
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65105|  192.168.255.15 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| Bfd | true |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF |
| -------- | --------- | --- |
| 172.31.255.96 | 65001 | default |
| 172.31.255.98 | 65001 | default |
| 172.31.255.100 | 65001 | default |
| 172.31.255.102 | 65001 | default |
| 192.168.255.1 | 65001 | default |
| 192.168.255.2 | 65001 | default |
| 192.168.255.3 | 65001 | default |
| 192.168.255.4 | 65001 | default |
| 123.1.1.10 | 1234 | Tenant_A_WAN_Zone |
| 123.1.1.11 | 1234 | Tenant_A_WAN_Zone |
| fd5a:fe45:8831:06c5::a | 12345 | Tenant_A_WAN_Zone |
| fd5a:fe45:8831:06c5::b | 12345 | Tenant_A_WAN_Zone |

### Router BGP EVPN Address Family

#### EVPN Host Flapping Settings

| State | Window | Threshold |
| ----- | ------ | --------- |
| Disabled | - |  - |

#### Router BGP EVPN MAC-VRFs

##### VLAN aware bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| Tenant_A_WAN_Zone | 192.168.255.15:14 | 14:14 | - | - | learned | 150 |
| Tenant_B_WAN_Zone | 192.168.255.15:21 | 21:21 | - | - | learned | 250 |
| Tenant_C_WAN_Zone | 192.168.255.15:31 | 31:31 | - | - | learned | 350 |

#### Router BGP EVPN VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Tenant_A_WAN_Zone | 192.168.255.15:14 | connected<br>static |
| Tenant_B_OP_Zone | 192.168.255.15:20 | connected |
| Tenant_B_WAN_Zone | 192.168.255.15:21 | connected |
| Tenant_C_WAN_Zone | 192.168.255.15:31 | connected |
| Tenant_L3_VRF_Zone | 192.168.255.15:15 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65105
   router-id 192.168.255.15
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor UNDERLAY-PEERS peer group
   neighbor UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor UNDERLAY-PEERS send-community
   neighbor UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.31.255.96 peer group UNDERLAY-PEERS
   neighbor 172.31.255.96 remote-as 65001
   neighbor 172.31.255.96 description DC1-SPINE1_Ethernet7
   neighbor 172.31.255.98 peer group UNDERLAY-PEERS
   neighbor 172.31.255.98 remote-as 65001
   neighbor 172.31.255.98 description DC1-SPINE2_Ethernet7
   neighbor 172.31.255.100 peer group UNDERLAY-PEERS
   neighbor 172.31.255.100 remote-as 65001
   neighbor 172.31.255.100 description DC1-SPINE3_Ethernet7
   neighbor 172.31.255.102 peer group UNDERLAY-PEERS
   neighbor 172.31.255.102 remote-as 65001
   neighbor 172.31.255.102 description DC1-SPINE4_Ethernet7
   neighbor 192.168.255.1 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.1 remote-as 65001
   neighbor 192.168.255.1 description DC1-SPINE1
   neighbor 192.168.255.2 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.2 remote-as 65001
   neighbor 192.168.255.2 description DC1-SPINE2
   neighbor 192.168.255.3 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.3 remote-as 65001
   neighbor 192.168.255.3 description DC1-SPINE3
   neighbor 192.168.255.4 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.4 remote-as 65001
   neighbor 192.168.255.4 description DC1-SPINE4
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle Tenant_A_WAN_Zone
      rd 192.168.255.15:14
      route-target both 14:14
      redistribute learned
      vlan 150
   !
   vlan-aware-bundle Tenant_B_WAN_Zone
      rd 192.168.255.15:21
      route-target both 21:21
      redistribute learned
      vlan 250
   !
   vlan-aware-bundle Tenant_C_WAN_Zone
      rd 192.168.255.15:31
      route-target both 31:31
      redistribute learned
      vlan 350
   !
   address-family evpn
      no host-flap detection
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor UNDERLAY-PEERS activate
   !
   vrf Tenant_A_WAN_Zone
      rd 192.168.255.15:14
      route-target import evpn 14:14
      route-target import evpn 65000:456
      route-target import vpn-ipv4 65000:123
      route-target export evpn 14:14
      route-target export evpn 65000:789
      route-target export vpn-ipv4 65000:123
      router-id 192.168.255.15
      neighbor 123.1.1.10 remote-as 1234
      neighbor 123.1.1.10 password 7 AQQvKeimxJu+uGQ/yYvv9w==
      neighbor 123.1.1.10 local-as 123 no-prepend replace-as
      neighbor 123.1.1.10 description External IPv4 BGP peer
      neighbor 123.1.1.10 ebgp-multihop 3
      neighbor 123.1.1.10 send-community standard extended
      neighbor 123.1.1.10 maximum-routes 0
      neighbor 123.1.1.10 default-originate
      neighbor 123.1.1.10 update-source Loopback123
      neighbor 123.1.1.10 route-map RM-Tenant_A_WAN_Zone-123.1.1.10-SET-NEXT-HOP-OUT out
      neighbor 123.1.1.10 route-map RM-123-1-1-10-IN in
      neighbor 123.1.1.11 remote-as 1234
      neighbor 123.1.1.11 password 7 AQQvKeimxJu+uGQ/yYvv9w==
      neighbor 123.1.1.11 local-as 123 no-prepend replace-as
      neighbor 123.1.1.11 description External IPv4 BGP peer
      neighbor 123.1.1.11 ebgp-multihop 3
      neighbor 123.1.1.11 send-community standard extended
      neighbor 123.1.1.11 maximum-routes 0
      neighbor 123.1.1.11 default-originate
      neighbor 123.1.1.11 update-source Loopback123
      neighbor 123.1.1.11 route-map RM-123-1-1-11-OUT out
      neighbor 123.1.1.11 route-map RM-123-1-1-11-IN in
      neighbor fd5a:fe45:8831:06c5::a remote-as 12345
      neighbor fd5a:fe45:8831:06c5::a send-community
      neighbor fd5a:fe45:8831:06c5::a route-map RM-Tenant_A_WAN_Zone-fd5a:fe45:8831:06c5::a-SET-NEXT-HOP-OUT out
      neighbor fd5a:fe45:8831:06c5::b remote-as 12345
      redistribute connected
      redistribute static
      !
      address-family ipv4
         neighbor 123.1.1.10 activate
         neighbor 123.1.1.11 activate
      !
      address-family ipv6
         neighbor fd5a:fe45:8831:06c5::a activate
         neighbor fd5a:fe45:8831:06c5::b activate
   !
   vrf Tenant_B_OP_Zone
      rd 192.168.255.15:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 192.168.255.15
      redistribute connected
   !
   vrf Tenant_B_WAN_Zone
      rd 192.168.255.15:21
      route-target import evpn 21:21
      route-target export evpn 21:21
      router-id 192.168.255.15
      redistribute connected
   !
   vrf Tenant_C_WAN_Zone
      rd 192.168.255.15:31
      route-target import evpn 31:31
      route-target export evpn 31:31
      router-id 192.168.255.15
      redistribute connected
   !
   vrf Tenant_L3_VRF_Zone
      rd 192.168.255.15:15
      route-target import evpn 15:15
      route-target export evpn 15:15
      router-id 192.168.255.15
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

### Router BFD Multihop Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

IGMP snooping is globally enabled.


### IP IGMP Snooping Device Configuration

```eos
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.168.255.0/24 eq 32 |
| 20 | permit 192.168.254.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.168.255.0/24 eq 32
   seq 20 permit 192.168.254.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

#### RM-Tenant_A_WAN_Zone-123.1.1.10-SET-NEXT-HOP-OUT

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | set ip next-hop 123.1.1.1 |

#### RM-Tenant_A_WAN_Zone-fd5a:fe45:8831:06c5::a-SET-NEXT-HOP-OUT

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | set ipv6 next-hop fd5a:fe45:8831:06c5::1 |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-Tenant_A_WAN_Zone-123.1.1.10-SET-NEXT-HOP-OUT permit 10
   set ip next-hop 123.1.1.1
!
route-map RM-Tenant_A_WAN_Zone-fd5a:fe45:8831:06c5::a-SET-NEXT-HOP-OUT permit 10
   set ipv6 next-hop fd5a:fe45:8831:06c5::1
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| Tenant_A_WAN_Zone | enabled |
| Tenant_B_OP_Zone | enabled |
| Tenant_B_WAN_Zone | enabled |
| Tenant_C_WAN_Zone | enabled |
| Tenant_L3_VRF_Zone | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance Tenant_A_WAN_Zone
!
vrf instance Tenant_B_OP_Zone
!
vrf instance Tenant_B_WAN_Zone
!
vrf instance Tenant_C_WAN_Zone
!
vrf instance Tenant_L3_VRF_Zone
```

# Platform

## Platform Summary

## Platform Configuration

```eos
!
platform sand lag hardware-only
```

# Quality Of Service