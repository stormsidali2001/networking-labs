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


