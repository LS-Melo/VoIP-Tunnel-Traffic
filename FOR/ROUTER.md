ip dhcp excluded-address 172.17.11.1
ip dhcp excluded-address 172.18.11.1

ip dhcp pool VLAN10
 
    network 172.17.11.0 255.255.255.0
    default-router 172.17.11.1
    dns-server 8.8.8.8

ip dhcp pool VLAN20
 
    network 172.18.11.0 255.255.255.0
    default-router 172.18.11.1
    option 150 ip 172.18.11.1
    dns-server 8.8.8.8
    
interface Tunnel200
 
    ip address 10.0.1.2 255.255.255.0
    mtu 1476
    tunnel source Serial0/0/0
    tunnel destination 194.65.3.2
    
interface FastEthernet0/0.10
 
    description Link para a rede de dados 17
    encapsulation dot1Q 10
    ip address 172.17.11.1 255.255.255.0
    ip nat inside

interface FastEthernet0/0.20
    
    description Link para a rede de voip18
    encapsulation dot1Q 20
    ip address 172.18.11.1 255.255.255.0
    ip nat inside
    
interface Serial0/0/0

    ip address 12.23.34.2 255.255.255.252
    ip nat outside
    
router eigrp 100
 
    passive-interface FastEthernet0/0
    passive-interface FastEthernet0/1
    network 172.17.11.0 0.0.0.255
    network 172.18.11.0 0.0.0.255
    network 10.0.1.0 0.0.0.255
    no auto-summary

ip nat inside source list 1 interface Serial0/0/0 overload

ip route 0.0.0.0 0.0.0.0 12.23.34.1 

access-list 1 permit 172.17.11.0 0.0.0.255

access-list 1 permit 172.18.11.0 0.0.0.255

dial-peer voice 1 voip
 
    destination-pattern 1..
    session target ipv4:10.0.0.1

dial-peer voice 2 voip
 
    destination-pattern 2..
    session target ipv4:192.168.1.1

telephony-service

    max-ephones 10
    max-dn 10
    ip source-address 172.18.11.1 port 2000
    auto assign 1 to 10

ephone-dn 1
 
    number 300

ephone-dn 2
 
    number 301
