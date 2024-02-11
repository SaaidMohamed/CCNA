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
SW1(config-if)#no cdp enable 

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
SW1(config)#lldp run                          (enable CDP all interfaces)
SW1(config)#interface fastEthernet0/6        (enable CDP specific interface)
SW1(config-if)#lldp transmit 
SW1(config-if)#lldp receive

```

-Disable LLDP:

```sh
SW1(config)#no lldp run                      (disable CDP all interfaces)
SW1(config)#interface fastEthernet0/6       (disable CDP specific interface)
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
Switch(config-if)#exit

```

-Configuring Trunk Ports:

```sh
Switch#configure terminal
Switch(config)#interface gigabitethernet 0/2
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk allowed vlan 10,20
Switch(config-if)#switchport access vlan 20  (Optional) Specify the default VLAN, which is used if the interface stops trunking.
Switch(config-if)#switchport trunk native vlan 20
Switch(config-if)#exit
Switch#show vlan
Switch#copy running-config startup config

```


-Troubleshooting VlANs:

```sh
SW1#show VLAN
SW1#show vlan brief 
SW1#show interfaces vlan 1
SW1#show interfaces trunk

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

```

## IPv6 Routing:

```sh
Router(config)#ipv6 unicast-routing                --->(enable IPV6 Routing)
Router(config)#interface GigabitEthernet0/0        --->(change to interface gig0)
Router(config-if)#ipv6 address fe80::1 link-local     --->(enable link-local IPv6)
Router(config)#ipv6 address 2001:db8:1:1::1/64     --->(enable global unicast address IPv6)
Router(config)#no shutdown                         --->(enable Interface)

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
Router(config-router)#area 0 range 10.1.0.0 255.255.0.0
Router(config-router)#area 1 range 10.0.0.0 255.255.0.0

- verify:
Router#show ip OSPF neighbors
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
