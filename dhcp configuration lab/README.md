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
c-default gateways
add 192.168.2.0.1/24 as the default gateway of pc2

add 192.168.1.0.1/24 as the default gateway of pc1

d-configuring the two points network 203.0.113.0/30:

router1(config):
int g0/0 > no shutdown > ip address 203.0.113.1 255.255.255.252
router2(config):
int g0/0 > no shutdown > ip address 203.0.113.1 255.255.255.252




