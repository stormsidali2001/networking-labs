1-setting up the network:
a-configuring  enhancing the routers with wic-2t phisical interface 
b-turning on the interfaces
c-setting  and ip address to each router interface:
note: a simple convension in the two point network the router with the heighest index
takes the highest ip address

router1:
int f0/0
ip address 10.0.0.1 255.255.255.0
no shutdown
int s0/3
ip address 15.0.0.1 255.255.255.252
no shutdown
int s0/0
ip address 16.0.0.2 255.255.255.252
no shutdown
int s0/1
ip address 17.0.0.1 255.255.255.252
no shutdown

router3:

int f0/0
ip address 13.0.0.1 255.255.255.0
no shutdown
int s3/0
ip address 15.0.0.2 255.255.255.252
no shutdown

int s2/0
ip address  14.0.0.2 255.255.255.252
no shutdown

router0:
int f0/0
ip address 12.0.0.1 255.255.255.0
no shutdown

int s0/3
ip address  14.0.0.1 255.255.255.252
no shutdown

int s0/0
ip address  16.0.0.1 255.255.255.252
no shutdown

int s0/2
ip address  18.0.0.1 255.255.255.252
no shutdown

router2:

int f0/0
ip address 11.0.0.1 255.255.255.0
no shutdown

int s0/1
ip address  18.0.0.2 255.255.255.252
no shutdown

int s0/0
ip address  17.0.0.2 255.255.255.252
no shutdown


2- now what are the default routes for each router:
router0:
C    12.0.0.0/8 is directly connected, FastEthernet0/0
     14.0.0.0/30 is subnetted, 1 subnets
C       14.0.0.0 is directly connected, Serial0/3
     16.0.0.0/30 is subnetted, 1 subnets
C       16.0.0.0 is directly connected, Serial0/0
     18.0.0.0/30 is subnetted, 1 subnets
C       18.0.0.0 is directly connected, Serial0/2
show ip route
router1:
show ip route
Gateway of last resort is not set

C    10.0.0.0/8 is directly connected, FastEthernet0/0
     15.0.0.0/30 is subnetted, 1 subnets
C       15.0.0.0 is directly connected, Serial0/3
     16.0.0.0/30 is subnetted, 1 subnets
C       16.0.0.0 is directly connected, Serial0/0
     17.0.0.0/30 is subnetted, 1 subnets
C       17.0.0.0 is directly connected, Serial0/1

router2:
show ip route
C    11.0.0.0/8 is directly connected, FastEthernet0/0
     17.0.0.0/30 is subnetted, 1 subnets
C       17.0.0.0 is directly connected, Serial0/0
     18.0.0.0/30 is subnetted, 1 subnets
C       18.0.0.0 is directly connected, Serial0/1

router3:
show ip route

C    13.0.0.0/8 is directly connected, FastEthernet0/0
     14.0.0.0/30 is subnetted, 1 subnets
C       14.0.0.0 is directly connected, Serial2/0
     15.0.0.0/30 is subnetted, 1 subnets
C       15.0.0.0 is directly connected, Serial3/0

configuring rip on each router:
router(config)
router rip
version 2 
no auto-sumarry 
note:
RIPv2 with the no auto-summary option will advertise the specific routes, not the aggregated prefix, unless you you use the ip summary-address rip interface command.
network address

router0(config)
router rip
version 2 
no auto-sumary



