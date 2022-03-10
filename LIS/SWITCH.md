interface FastEthernet0/1
 
    switchport mode trunk

interface FastEthernet0/2
 
    switchport access vlan 10
    switchport mode access

interface FastEthernet0/3

    switchport access vlan 10
    switchport mode access
    switchport voice vlan 20
    mls qos trust cos
