ip dhcp excluded-address 192.168.0.1

ip dhcp excluded-address 192.168.1.1

ip dhcp pool VLAN10
 
     network 192.168.0.0 255.255.255.0
     default-router 192.168.0.1
     dns-server 8.8.8.8

ip dhcp pool VLAN20

    network 192.168.1.0 255.255.255.0
    default-router 192.168.1.1
    option 150 ip 192.168.1.1
    dns-server 8.8.8.8

interface Tunnel100

    ip address 10.0.0.2 255.255.255.252
    mtu 1476
    tunnel source Serial0/0/1
    tunnel destination 94.65.3.2

interface Tunnel200

    ip address 10.0.1.1 255.255.255.0
    mtu 1476
    tunnel source Serial0/0/1
    tunnel destination 12.23.34.2
    
interface FastEthernet0/0.10
 
    encapsulation dot1Q 10
    ip address 192.168.0.1 255.255.255.0
    ip nat inside

interface FastEthernet0/0.20
 
    encapsulation dot1Q 20
    ip address 192.168.1.1 255.255.255.0
    ip nat inside
    
interface Serial0/0/1
 
    ip address 194.65.3.2 255.255.255.252
    ip nat outside
    
router eigrp 100
 
    passive-interface FastEthernet0/0
    network 10.0.0.0 0.0.0.3
    network 192.168.0.0
    network 192.168.1.0
    network 10.0.1.0 0.0.0.255
    no auto-summary

ip nat inside source list 1 interface Serial0/0/1 overload

ip route 0.0.0.0 0.0.0.0 194.65.3.1 

access-list 1 permit 192.168.0.0 0.0.255.255

dial-peer voice 1 voip
 
    destination-pattern 1..
    session target ipv4:172.16.1.1

dial-peer voice 2 voip
    
    destination-pattern 3..
    session target ipv4:172.18.11.1

telephony-service
     
    max-ephones 10
    max-dn 10
    ip source-address 192.168.1.1 port 2000
    auto assign 1 to 10

ephone-dn 1
 
    number 200

ephone-dn 2

    number 201
