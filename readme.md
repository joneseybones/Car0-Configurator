# Car0-Configurator
Ansible Playbook used to auto-configure a Car0 Switch and AP for testing. 
Takes the testing AP hostname as an input, and parses the hostname to determine the position on the train.
The position will specify how to configure both the Car0 switch and AP, based on the following rules:
- The Car0 switch will have a static IP and push DHCP if the testing AP is **train rear**
- The Car0 switch will receive an IP through DHCP if the testing AP is **train front**
- The Car0 AP will act as root bridge if testin AP is **train rear**, and non-root bridge when testing **train front** APs
- The Car0 AP will update the SSID (RIGHT or LEFT) based on the side of the train the testing AP is on

For example, when testing an *rf* AP position, the switch with configure its AP-facing VLAN interface (VLAN 66) to pull IPv4 DHCP, and the AP will run in non-root bridge mode with the channel and SSID information for *train left*

The ansible script pauses to allow the connection to come up and the OSPF neighborship to form, then dump the output of the "show dot11 association all-client" and relevant switch and AP configuration to file
