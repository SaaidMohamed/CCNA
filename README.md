# [CCNA Configuration Guide][pypi-url]

[pypi-url]: https://www.cisco.com/c/en/us/training-events/training-certifications/certifications/associate/ccna.html


## Description:
[CCNA Configuration Guide](https://www.cisco.com/c/en/us/training-events/training-certifications/certifications/associate/ccna.html) is a basic Cisco IOS Configuration reference guide for configuring Cisco switches and routers. 


## Initial Configuration:

+name the device:

```sh
SW1(config)#hostname AccessSwitch1

```
*secure enable prompt:

```sh
AccessSwitch1(config)#enable secret somePassword

```

+SVI Managment IP:

```sh
SW1(config)#     interface vlan1
SW1(config-if)#  ip address 10.10.10.2 255.255.255.0
SW1(config-if)#  no shutdown
SW1(config-if)#  exit
SW1(config)#     ip default-gateway 10.10.10.2

```

## CDP (Cisco Discovery Protocol):

-Enable CDP:

```sh
SW1(config)#cdp run                          (enable CDP all interfaces)
SW1(config)#interface fastEthernet0/6        (enable CDP specific interface)
SW1(config-if)#cdp enable 

```

-Disable CDP:

```sh
SW1(config)#no cdp run                      (disable CDP all interfaces)
SW1(config)#interface fastEthernet0/6       (disable CDP specific interface)
SW1(config-if)#no cdp enable 

```
-Troubleshooting CDP:

```sh
SW1#show cdp
SW1#show cdp neighbors
SW1#show cdp neighbors detail 

```

## LLDP (link layer discovery protocol):

-Enable LLDP:

```sh
SW1(config)#lldp run                          (enable LLDP all interfaces)
SW1(config)#interface fastEthernet0/6        (enable LLDP specific interface)
SW1(config-if)#lldp transmit 
SW1(config-if)#lldp receive

```

-Disable LLDP:

```sh
SW1(config)#no lldp run                      (disable LLDP all interfaces)
SW1(config)#interface fastEthernet0/6       (disable LLDP specific interface)
SW1(config-if)#no lldp transmit 
SW1(config-if)#no lldp receive

```

-Troubleshooting LLDP:

```sh
SW1#show lldp
SW1#show lldp neighbors
SW1#show lldp neighbors detail 

```

## VLANs (Virtual Local Area Network):

-Create VLANs:

```sh
Switch#configure terminal
Switch(config)#vlan 20
Switch(config-vlan)#exit

```

-Delete VLANs:

```sh
Switch#configure terminal
Switch(config)#no vlan 20
Switch(config-vlan)#exit

```

-Assigning Static-Access Ports to a VLAN:

```sh
Switch#configure terminal
Switch(config)#interface gigabitethernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#switchport voice vlan 50 (Optional) for ip phone ports only .
Switch(config-if)#exit

```


-Configuring Trunk Ports:

```sh
Switch#configure terminal
Switch(config)#interface gigabitethernet 0/2
Switch(config-if)#description Trunk to SW3                    (Optional) for Troubleshooting purposes only.
Switch(config-if)#switchport trunk encapsulation dot1q        (Optional) for legacy switches only.
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config-if)#switchport access vlan 20  (Optional) Specify the default VLAN, which is used if the interface stops trunking.
Switch(config-if)#switchport trunk native vlan 20
Switch#copy running-config startup config


-DTP (Dynamic Trunking Protocol)
Switch(config)#interface gigabitethernet 0/2
Switch(config-if)#switchport mode dynamic auto (will form a trunk only if other side is trunk or desirable)
Switch(config-if)#switchport mode dynamic desirable (will form a trunk in all cases, defaults in legacy switches)
Switch(config-if)#switchport nonegotiate (disable DTP)


-VTP (VLAN Trunking Protocol) to centerlize one switch as a server for creating and deleting VLANs
Switch(config)#vtp domain DomainName
Switch(config)#vtp mode server         (default, only switch with higher revision number)
Switch(config)#vtp mode client         (gets vlan updates from server with higher revision number)
Switch(config)#vtp mode transparent    (only passes changes between server and client)

-Verify VTP:
SW1#show vtp status

```

-Verify VlANs:

```sh
SW1#show VLAN
SW1#show vlan brief 
SW1#show interfaces 0/1 switchport 
SW1#show interfaces vlan 1
SW1#show interfaces trunk

```

## Inter-VLAN Routing:

-Router with Separate Interfaces:
```sh
Router(config)#interface gigabitEthernet0/0/0
Router(config-if)#ip address 10.10.10.1 255.255.255.0
Router(config-if)#no shutdown

Switch(config)#interface gigabitethernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20

```

-Router on a Stick:
```sh
Router(config)#interface gigabitEthernet0/0/0     ( bring router interface gig0 up )
Router(config-if)#no shut
Router(config-if)#exit

Router(config)#interface gigabitEthernet0/0/0.1    ( create subinterface for Native VLAN with dot1q)
Router(config-subif)#encapsulation dot1Q 188
Router(config-subif)#encapsulation dot1Q 188 native 
Router(config-subif)#exit

Router(config)#interface gigabitEthernet0/0/0.10    ( create subinterfaces for each VLAN with dot1q VLAN 10 in this example)
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 10.0.0.1 255.255.255.0
Router(config-subif)#exit

Switch(config)#interface gigabitethernet 0/1  (uplink from Switch to router)
Switch(config-if)#switchport trunk encapsulation dot1Q
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk native vlan 188

```


-L3 Switch Inter-VLAN:
```sh
Switch(config)#ip routing
Switch(config)#interface vlan 10
Switch(config-if)#ip address 10.0.10.1 255.255.255.0
Switch(config)#interface vlan 20
Switch(config-if)#ip address 10.0.20.1 255.255.255.0

Switch(config)#interface gigabitethernet 0/1  (uplink from Switch to router)
witch(config-if)#no switchport
Switch(config-if)#ip address 10.0.50.1 255.255.255.0
Switch(config)#ip 0.0.0.0 0.0.0.0 10.0.50.2 

```

## Etherchannel:

-LACP (Link Aggregation Control Protocol "active/passive")
```sh
Switch(config)#interface range gigabitEthernet0/1-2
Switch(config-if-range)#channel-group 1 mode active

-Configure Grouped Interfaces:
Switch(config)#interface port-channel 1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20

-EtherChannel Load-Balancing:
Switch(config)# port-channel load-balance dst-mac

```


-PAgP (Port Aggregation Protocol "desirable/auto")

```sh
Group Interfaces:
Switch(config)#interface range gigabitEthernet0/1-2
Switch(config-if-range)#channel-group 1 mode desirable

Configure Grouped Interfaces:
Switch(config)#interface port-channel 1
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20

EtherChannel Load-Balancing:
Switch(config)# port-channel load-balance dst-mac

-Layer 3 Etherchannel:
Switch(config)#interface range gigabitEthernet0/1-2
Switch(config-if-range)# no switchport
Switch(config-if-range)#channel-group 1 mode  active | desirable | on

Switch(config)#interface port-channel 1
Switch(config-if)#ip address 10.10.10.1 255.255.255.0
Switch(config-if)#no shutdown
```

## STP (Spanning-Tree Protocol):
```sh
-Bridge ID: (either MAC Address or 0-65535, default is 32768)
-Root Bridge: is a switch with lowest ID (priority"32768" or MAC address)
-cost:(10Mbps:100, 100Mbps:19, 1Gbps: 4, 10Gbps:2, 20Gbps:1)
-each switch has only one root port.
-all ports on rootbridge are Designated ports (looking away from rootbridge).
-only BPDUs traffic is sent on the blocking links. for failover
-802.1d(1STP for all VLANs)/802.1w(1RSTP for all VLANs)/802.1s(MSTP, loadbalancing,groups stp instances)
-PVST+(per VLAN Spanning-tree plus): Cisco inhancement to 802.1d, STP per vlan but still slow does not group.
-RPVST+(Rapid per VLAN Spanning-tree plus):Cisco inhancement to 802.1w,STP per vlan & convergence time is good does not group.

Switch(config)#spanning-tree pathcost method long/short

-Select Root Bridge:
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#spanning-tree cost 1                 (change cost of an interface)
Switch(config-if)#spanning-tree VLAN 1 root primary    (select primary root)
witch(config-if)#spanning-tree VLAN 1 root secondary   (select backup root)
witch(config-if)#spanning-tree VLAN 1 priority 0-61440 (increment of 4096)

-Enable Portfast & BPDUguard:
Switch(config)#spanning-tree portfast default (all interfaces,to bring interface up for end hosts )
Switch(config)#spanning-tree bpduguard default (all interfaces,to detect switch instead of end host)

Switch(config)#interface fastEthernet 0/1 (specific interface)
Switch(config-if)#spanning-tree portfast
Switch(config-if)#spanning-tree bpduguard enable

-Enable RootGuard:(make sure no other switch becomes root bridge)
Switch(config)#interface fastEthernet 0/1 (configure all interfaces except secondary rootbridge interf)
Switch(config-if)#spanning-tree guard root





-Verify:
Switch#show spanning-tree pathcost method
Switch#show spanning-tree
Switch#show spanning-tree VLAN 1

```

## Port Security Configuration:

```sh
-port Security action to violation modes: shutdown/protect/restrect

Switch(config)#interface fastEthernet 0/1 (interface to limit to only one MAC address)
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-Security
Switch(config-if)#switchport port-Security violation protect (change port security action mode)
Switch(config-if)#switchport port-Security maximum 2 (2 MAC addresses)
Switch(config-if)#switchport port-Security mac-address  444.222.111 (only specific mac address)
Switch(config-if)#switchport port-Security mac-address  sticky (learned mac as secure mac)



-verify:
Switch# show port-Security interface fa0/1
Switch# show port-Security 
Switch# show port-Security address

```

## IPv6 Routing:

```sh
-ipv6 Global Unicast addresses (are 128 bits public addresses, start from 2000:::::::/48 and 
rest 16 bits are for subnets other half 64bits are for host )

Router(config)#ipv6 unicast-routing                --->(enable IPV6 Routing)
Router(config)#interface GigabitEthernet0/0        --->(change to hosts interface gig0)
Router(config-if)#ipv6 address fe80::1 link-local  --->(enable link-local IPv6)
Router(config)#ipv6 address 2001:db8:1:1::1/64     --->(enable global unicast address IPv6 for public conectivity)
Router(config)#no shutdown                         --->(enable Interface)


-EUI-64 IPv6 Addresses (Configure host to generate interface ip using their MAC address 
and adding FF:FE in the middle of MAC to get to 64bits and invert 7th bit.)

Router(config)#interface Gig 0/1
Router(config-if)#ipv6 address 2001:db8:0:1::/64 eui-64   (it will look:2001:db8:0:1:"c"801:2F"FF:FE"24:0
                                                  C is 7th bit inverted and FF:FE added in middle )

-Unique Local IPv6 Addresses (are like IPv4 Private addresses, with range FC00::/7 and host are assigned FC00::1/64 mask)


-Link Local IPv6 Addresses (starts with FE80::/10 - FEB0::/10, hosts should have /64 ipv6 address. can be used for 
routing protocols hello packet and updates or any communication that shouldn't be forwarded beyond local link)
_they are automatically generated as EUI-64 Link Local with mac in the middle when ipv6 is enabled on an inteface but can manually overwriten:

Router(config)#interface Gig 0/1
Router(config-if)#ipv6 address FE80::1/64 link-local 

-SLAAC(Stateless Address AutoConfiguration)
(-when enabling an IPv6 Adrress on an interface it starts sending advertissment with network prfex and default gateway 
address to hosts using ICMP and Multi-cast address.
-Hosts will use router advertisments to generate their EUI-64 IPv6 address using thier mac or random mac. DHCP server is still needed for DNS ip
-Host can also rquest an IPv6 address by sending Router Solicitation message to the router.
)
-ND (Neighbor Discovery):
Neighbor Discovery is equivalant of ARP and use ICMP Neighbor Solicitation and Neighbor Advertisment to multicast addresses.


Verify:
Router# show ipv6 interface brief
Router# show ipv6 Neighbors

```


## Configure IP Addresses on Routers' Interfaces:

```sh
Router(config)#interface gigabitEthernet0/0/0
Router(config-if)#ip address 10.10.10.1 255.255.255.0
Router(config-if)#no shutdown

```

## Configure Static Routes on a Router:

```sh
Router(config)#ip route 10.10.10.0 255.255.255.0 10.10.1.1 
Router(config)#ip route 10.10.11.0 255.255.255.0 10.10.1.1 

```

## Configure RIP (120/HOPS):

```sh
Router#configure terminal 
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 10.0.0.0
Router(config-router)#no auto-summary 
Router(config-router)#passive-interface loopback 0
Router(config-router)#passive-interface fastEthernet0/1

Router(config)#interface fastEthernet0/1 (interface we send summaries out of)
Router(config-if)#ip summary-address rip 10.0.0.0 255.255.0.0

-inject default Static route with RIP:
Router(config)# ip route 0.0.0.0  0.0.0.0 203.0.113.2 (to the internet interface)
Router(config)#router rip
Router(config-router)# default-information originate

- verify:
Router#show ip protocols 
Router#show run | section rip
Router#show ip route
Router#show ip rip database

```

## Configure IS-IS (115/HOPS&COST --> manually configure bandwidth on interfaces for IS-IS to use cost):

```sh
Router#configure terminal 
Router(config)#router isis
Router(config-router)#network 49.0001.0000.0000.0003.00
Router(config)#interface gigabitEthernet0/0/0
Router(config-if)#ip router isis
Router(config)#interface gigabitEthernet0/0/1
Router(config-if)#ip router isis

```

## Configure OSPF(110/COST):

```sh
Router#configure terminal 
Router(config)#router OSPF 1
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
Router(config-router)#router-id 2.2.2.2 (use configured ip on the router)

Router(config-router)#passive-interface loopback 0 (passive int will be advertised but wont form any adjacencies)
Router(config-router)#passive interface fastEthernet0/3 (interface to ISP we don't want it to form any adjacencies)


-inject default Static route with OSPF:
Router(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2 (to the internet interface)
Router(config)#router OSPF 1
Router(config-router)# default-information originate


-ABR(area border router) summarization:
Router(config)#router OSPF 1
Router(config-router)#network 10.1.0.0 0.0.255.255 area 0
Router(config-router)#network 10.0.0.0 0.0.255.255 area 1
Router(config-router)#area 0 range 10.1.0.0 255.255.0.0 (summarize above networks)
Router(config-router)#area 1 range 10.0.0.0 255.255.0.0 (summarize above networks)


-bandwidth reference:
Router(config)#router OSPF 1
Router(config-router)#auto-cost reference-bandwidth 100000 (from default 100 to 100000 to prefer 1Gig+ interfaces)


-prfered way of manupulating ospf cost:
Router(config)#interface GigabitEthernet0/0  
Router(config-if)#ip ospf cost 60


-ospf hello-interval:
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip ospf hello-interval 5
Router(config-if)#ip mtu 1460

-Multi-Access Segment (Multiple routers connected to a switch), 
    OSPF Priority to elect DR(Designated Router,224.0.0.6) & BDR(Backup Designated Router) 
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip ospf Priority 100    (0-255 if 0 router will never be a DR,
for BDR on a different interface any number less than 100 and greater than 1. restart needed)


- verify:
Router#sh
ow ip OSPF neighbors
Router#show ip protocols 
Router#show run | section OSPF
Router#show ip route
Router#show ip OSPF database

```

## Configure EIGRP(90/COST):

```sh
Router#configure terminal 
Router(config)#router EIGRP 100
Router(config-router)#no auto-summary 
Router(config-router)#network 10.0.0.0 0.255.255.255 
Router(config-router)#EIGRP router-id 2.2.2.2

-verify:
Router#show ip protocols 
Router#show run | section EIGRP
Router# show ip route
Router#show ip EIGRP database
Router#show ip EIGRP neighbors

```

## Interface speed:

```sh
-Ethernet interfaces:
Router(config)#interface GigabitEthernet0/0     
Router(config-if)#speed 100        (from default 1000Mbps to 100Mbps)

-Serial Interfaces:
Router(config)#interface Serial 1/0    
Router(config-if)#clock rate 64000 (from default 1.544Mbps to 64kbps)

```

## Interface bandwidth (does not change physical speed):

```sh
-Ethernet interfaces:
Router(config)#interface GigabitEthernet0/0     
Router(config-if)#bandwidth 100        (from default 1000Mbps to 100Mbps)

-Serial Interfaces:
Router(config)#interface Serial 1/0    
Router(config-if)#bandwidth 768 (from default 1.544Mbps to 768)

```

## DHCP on a Router:

```sh
R1(conf)#       ip dhcp excluded-address 10.10.10.1 10.10.10.10   (range of excluded ip address)
R1(conf)#       ip dhcp pool Flackbox
R1(conf-if)#    network 10.10.10.0 255.255.255.0 (network of included ip address)
R1(conf-if)#    default-router 10.10.10.1  (default gateway of the clients)
R1(conf-if)#    dns-server 10.10.20.15     (dns server of the clients)

-Verify:
R1#             show ip dhcp pool
R1#             show dhcp binding

```

## DHCP Server on a Different Network:
```sh
Router(config)#interface GigabitEthernet0/0  (interface connected to clients requesting ip)
Router(config-if)# ip helper-address 10.10.30.5 (point clients to dhcp server ip)

```
## Configure Router Interf as DHCP Client:
```sh
Router(config)#interface fastEthernet0/1  (interface connected to ISP to get IP)
Router(config-if)#ip address dhcp
Router(config-if)#no shutdown

-verify:
R1# show dhcp lease

```

## DHCP Snooping on Switches: (for switch to only trust requests coming from specific DHCP interface)
```sh
Switch(config)#ip dhcp Snooping            (enable globally)
Switch(config)#ip dhcp Snooping vlan 20    (enable on vlan)
Switch(config)# interface fa0/3            (interface coming from dhcp)
Switch(config-if)#ip dhcp Snooping trust

```

## DAI (Dynamic ARP Inspection): (Works with DHCP snooping to map ip provided by dhcp to MAC address 
                                      to prevent ARP Spoofing/Poisoning or DOS attack)
```sh
Switch(config)#ip arp inspection vlan 20    (enable on a vlan level to protect all hosts)

Switch(config)# interface fa0/3            (hosts with static ip, or any host that doesn't recieve ip from dchp can't 
                                             be protected by DAI and need to be excluded & configured to trust arp)
Switch(config-if)#ip arp inspection trust

```


## FHRP (First Hop Router Protocol):
```sh
-HSRP(Hot Standby Router Protocol)  (active/Standby Pair):
Router1(config)#interface gig0/1 
Router1(config-if)#ip address 10.10.10.2 255.255.255.0
Router1(config-if)#no shutdown
Router1(config-if)#standby 1 ip 10.10.10.1 (to load balance create 2 group with same config except Priority)
Router1(config-if)#standby 1 Priority 110 (default 100, this router will be active, 
                                           if priority not configured highest ip address will be active)
Router1(config-if)#standby 1 Preempt (preffered not to preempt)
Router1(config-if)#standby version 2


Router2(config)#interface gig0/1
Router2(config-if)#ip address 10.10.10.3 255.255.255.0
Router2(config-if)#no shutdown
Router2(config-if)#standby 1 ip 10.10.10.1(to load balance create 2 group with same config except Priority)
Router1(config-if)#standby 1 Priority 90 (default 100, this router will be standby)
Router1(config-if)#standby version 2

-Verify:
R1# show standby


-VRRP(Virtual Router Redundancy Protocol)  (active/Standby Pair):

-GLBP(Gateway Load Balancing Protocol)  (active/active Pair):

```


HSRP and STP with Router on a Stick :
```sh

Router1(config)#interface gig0/1.10
Router1(config)#encapsulation dot1Q vlan 10
Router1(config-if)#ip address 10.10.10.2 255.255.255.0
Router1(config-if)#no shutdown
Router1(config-if)#standby 1 ip 10.10.10.1
Router1(config-if)#standby 1 Priority 110 
Router1(config-if)#standby 1 Preempt 

Router1(config)#interface gig0/1.20
Router1(config)#encapsulation dot1Q vlan 20
Router1(config-if)#ip address 10.10.20.2 255.255.255.0
Router1(config-if)#no shutdown
Router1(config-if)#standby 1 ip 10.10.10.1
Router1(config-if)#standby 1 Priority 90 

Switch1(config)#spanning-tree vlan 10 root primary
Switch1(config)#spanning-tree vlan 20 root secondary



Router2(config)#interface gig0/1.10
Router2(config)#encapsulation dot1Q vlan 10
Router2(config-if)#ip address 10.10.10.3 255.255.255.0
Router2(config-if)#no shutdown
Router2(config-if)#standby 1 ip 10.10.10.1
Router2(config-if)#standby 1 Priority 90


Router2(config)#interface gig0/1.20
Router2(config)#encapsulation dot1Q vlan 20
Router2(config-if)#ip address 10.10.10.3 255.255.255.0
Router2(config-if)#no shutdown
Router2(config-if)#standby 1 ip 10.10.20.1
Router2(config-if)#standby 1 Priority 110 
Router2(config-if)#standby 1 Preempt 

Switch2(config)#spanning-tree vlan 20 root primary
Switch2(config)#spanning-tree vlan 10 root secondary

```


ACLS (Access Control Lists) :
```sh
-Standard ACLS : 1-99/ 1300-1999 (checks only source IP address)
Router1(config)#access-list 1 deny 10.10.10.11 0.0.0.0 
Router1(config)#access-list 1 permit 10.10.10.0 0.0.0.255
Router2(config-std-nacl)# permit ip any       (to overwrite excplecit deny)

-Extended ACLS : 100-199 /2000-2699 (checks source/destination/tcp/udp/ports)
Router2(config)#access-list 100 deny tcp 10.10.10.5 0.0.0.0 gt 49151 10.10.20.40 0.0.0.0 eq 22
Router2(config)#access-list 100 permit tcp 10.10.10.0 0.0.0.255 gt 49151 10.10.20.40 0.0.0.0 eq 22

-Named ACLs (Standard & Extended):
Router2(config)# ip access-list standard ACLName
Router2(config-std-nacl)# deny 10.10.10.11 0.0.0.0 
Router2(config-std-nacl)# permit 10.10.10.11 0.0.0.0 
Router2(config-std-nacl)# permit ip any       (to overwrite excplecit deny)

Router2(config)# ip access-list extended ACLName
Router2(config-std-nacl)# deny tcp 10.10.10.5 0.0.0.0 gt 49151 10.10.20.40 0.0.0.0 eq 22
Router2(config-std-nacl)# permit tcp 10.10.10.0 0.0.0.255 gt 49151 10.10.20.40 0.0.0.0 eq 22
Router2(config-std-nacl)# permit ip any any       (to overwrite excplecit deny)


-wildcards:
Router1(config)#access-list 1 deny host 10.10.10.11 
Router2(config)#access-list 100 permit tcp any

-Apply access lists to interfaces:
-only one ACL inbound interface and one outbound interface.

Router2(config)#interface gig0/1
Router2(config-if)# ip access-group 100 out
Router2(config-if)# ip access-group 103 in

-inject an ACL between existing ones:
Router2(config)# ip access-list extended ACL_Name
Router2(config-std-nacl)# 25 deny tcp 10.10.10.5 0.0.0.0 gt 49151 10.10.20.40 0.0.0.0 eq 22



-Verify:
R1# show access-list 100
R1# show ip interface gig0/1 | include access list

```


NAT (Network Address Translation):
```sh
-Static NAT:
-- first determine inside interface and outside interface:

Router1(config)# interface fa0/1
Router1(config-if)# ip nat outside 

Router1(config)# interface fa0/2
Router1(config-if)# ip nat inside

--then translate in global config
Router1(config)#ip nat inside source static 10.0.10.10 203.0.114.4



-Dynamic NAT:
-- first determine inside interface and outside interface:

Router1(config)# interface fa0/1
Router1(config-if)# ip nat outside 

Router1(config)# interface fa0/2
Router1(config-if)# ip nat inside

--then configure a pool of public ip addresses for Dynamic NAT to choose from
Router1(config)# ip nat pool PoolName 203.0.144.4 203.0.144.15 netmask 255.255.255.240

--then create ACL (access list) for internal IP addresses to translate to public ip addresses:
Router1(config)#access-list 1 permit 10.10.10.0  0.0.0.255

--finally, link access list with NAT pool to complete dynamic NAT Config:
Router1(config)#ip nat inside source list 1 pool PoolName



-Dynamic NAT Overload (PAT) (Port Address Translation) 
                      same config as NAT except adding overload at the end:

-- first determine inside interface and outside interface:
Router1(config)# interface fa0/1
Router1(config-if)# ip nat outside 

Router1(config)# interface fa0/2
Router1(config-if)# ip nat inside

--then configure a pool of public ip addresses for Dynamic NAT overload to choose from:
Router1(config)# ip nat pool PoolName 203.0.144.4 203.0.144.15 netmask 255.255.255.240

--then create ACL (access list) for internal IP addresses to translate to public ip addresses and ports:
Router1(config)#access-list 1 permit 10.10.10.0  0.0.0.255

--finally, link access list with NAT pool to complete dynamic NAT Overload Config:
Router1(config)#ip nat inside source list 1 pool PoolName overload


-Verify NAT:
R1# show ip nat translation
R1# show ip nat statistics
R1# clear ip nat translations (remove translations from table to edit nat config)
R1# clear ip nat translations * (will remove all dynamic nat translations)


```