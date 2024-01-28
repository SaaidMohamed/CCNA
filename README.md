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







<!---


## Features:
* Create new files on your deskop by saying `sesame <wait for indicator> create file named <filename.extension>`.
* Delete existing files on your deskop by saying `sesame <wait for indicator> delete file named <filename.extension>`.
* Create new Folders on your deskop by saying `sesame <wait for indicator> create folder named <foldername>`.
* Delete existing Folders on your deskop by saying `sesame <wait for indicator> delete folder named <foldername>`.
* Close any application on your PC by saying `sesame <wait for indicator> close <app name>`.
* Open any application on your PC by saying `sesame <wait for indicator> open <app name>`.
* Search anything on Google by saying `sesame <wait for indicator> sarch Google <Query>`.
* Search anything on Youtube by saying `sesame <wait for indicator> sarch Youtube <Query>`.
* Sign out of your account by saying `sesame <wait for indicator> sign out`.
* Shutdown your computer by saying `sesame <wait for indicator> shutdown computer`.
* Restart your computer by saying `sesame <wait for indicator> restart computer`.


## Installation:
Install playsound:
```sh
pip install playsound==1.2.2
```
Install AppOpener:
```sh
pip install AppOpener
```
Install speech_recognition:
```sh
pip install speech_recognition
```
Install gTTS:
```sh
pip install gTTS
```
Install pocketsphinx:
```sh
pip3 install pocketsphinx
```
In project.py, file in create_file(), delete_file(), create_folder() and delete_folder() functions make sure you choose convenient path where you want to create/delete files, and change listen-start-sound.mp3 path or it will default to speaking "recording" as an indicator to start recording.


## Requirements:
To use all of the functionality of the program, you should have installed:

* **Python** 3.8+ (required)
* **playsound** 1.2.2 (required)
* **AppOpener** 1.7+ (required)
* **speech_recognition** 3.10.0+ (required)
* **gTTS**  2.4.0+ (required)
* **pocketsphinx** 5.0.2+ (required)
* **PyAudio**    (required)




## More Information:
For more information check out these libraries :
- [SpeechRecognition 1.2.4](https://pypi.org/project/SpeechRecognition/1.2.4/)
- [pocketsphinx 5.0.2](https://pypi.org/project/pocketsphinx/)
- [appopener 1.7](https://pypi.org/project/appopener/)
- [gTTS 2.4.0](https://pypi.org/project/gTTS/)
- [playsound 1.3.0](https://pypi.org/project/playsound/)
- [webbrowser](https://docs.python.org/3/library/webbrowser.html)
- [os](https://docs.python.org/3/library/os.html)



## Author:
Mohamed Saaid

--->


