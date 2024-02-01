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
