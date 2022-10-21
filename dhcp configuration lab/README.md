todos:
1- creating the following pools:
POOL1:192.168.1.0/24  (preserve .1 to .10)
  DNS:8.8.8.8
  domain:assoulsidali1.com
  default gateway:R1

POOL2:192.168.2.0/24  (preserve .1 to .10)
  DNS:8.8.8.8
  domain:assoulsidali2.com
  default gateway:R2
  
POOL3:203.0.113.0/30  (preserve .1 )

2-configure R1's g0/0 interface as DHCP client What ip address did it configure ?
3-configure R1 as DHCP relay agent for the 192.168.1.0/24 subnet
4-use the cli of pc1 and pc2 to make them request an ip address from their dhcp server.



how to proceed:
1-setting up the architecture
a-go to R2 and type the following commands:
enable > configure terminal > int g0/1 > no shutdown > ip address 192.168.2.1 255.255.255.0

b-go to R1 and type the following commands:
enable > configure terminal > int g0/1 > no shutdown > ip address 192.168.1.1 255.255.255.0

note: you can check the stat of the interfaces using the command: do show ip interfaces brief


c-configuring the two points network 203.0.113.0/30:

router1(config):
int g0/0 > no shutdown > ip address 203.0.113.1 255.255.255.252
router2(config):
int g0/0 > no shutdown > ip address 203.0.113.1 255.255.255.252


configuring dhcp:

router2(config):
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp excluded-address 192.168.2.1 192.168.2.10
ip dhcp excluded-address 203.0.113.2 (the dhcp server R2)

ip dhcp pool POOL1
router(dhcp-config):
network 192.168.1.0 255.255.255.0
dns-server 8.8.8.8
domain-name assoulsidali1.com
default-router 192.168.1.1 (R1)
exit

ip dhcp pool POOL2
network 192.168.2.0 255.255.255.0
dns-server 8.8.8.8
domain-name assoulsidali2.com
default-router 192.168.2.1 (R2)
exit

ip dhcp POOL3 
network 192.168.2.0 255.255.255.252
exit

note: to show all the dhcp command which you've types use the command:
do show run | section dhcp

//to check if pc2 can get an address from r2
go to pc2 command prompt:
ipconfig  /renew
the result should be :
 IP Address......................: 192.168.2.11
 Subnet Mask.....................: 255.255.255.0
 Default Gateway.................: 192.168.2.1
 DNS Server......................: 8.8.8.8
 
 for more details like domain-name ....
 type: ipconfig /all
 
 if you ty the same steps for pc1 you will get an error when entering the command: ipconfig /renew (we have to configure r1 g0/0 interface as a dhcp client to resolve that)

2-
2-configure R1's g0/0 interface as DHCP client:
router1(config)
int g0/0
ip address dhcp
exit
//now r1 will broadcast a dhcp discover message to r2
//r2 reply with a dhcp offer
//then r1 sends a dhcp request
//finally r2 sends a dhcp ack
