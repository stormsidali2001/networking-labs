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
2-
