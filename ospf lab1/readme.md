# Ospf Lab1
### Configuration Steps

* [x] setting up the network structure
  * [x] enable routers interfaces
  * [x] assigning ip addresses to all the interfaces
* [x] configuring a loop back interface on each router 1.1.1.1/32 for R1 2.2.2.2 for R2 etc...
* [x] configure ospf on each router (including loopback interfaces)  (do not enable ospf on router1 internet link)
* [x] configure passive interfaces when appropriate including loopback interfaces.
* [x] configure R1 as an ASBR that advertises a default route into the ospf domain
* [x] check the routing tables of R2 , R3 and R4 what default routes were added.

### enable routers interfaces && assigning ip addresses to all the interfaces
>open the router cli and type: **enable** \> **configure terminal** \> then you will be located at : hostname(config)
1. router1
```
int G0/0 
ip address 10.0.13.1 255.255.255.252
no shutdown
```
```
int G0/1
ip address 10.0.13.1 255.255.255.252
no shutdown
```
```
int G0/2
ip address 203.0.113.1 255.255.255.252
no shutdown
```
2. router2
```
int G0/0 
ip address 10.0.12.2 255.255.255.252
no shutdown
```
```
int G0/1
ip address 10.0.24.1 255.255.255.252
no shutdown
```
3. router3
```
int G0/0 
ip address 10.0.13.2 255.255.255.252
no shutdown
```
```
int G0/1
ip address 10.0.34.1 255.255.255.252
no shutdown
```

4. router4
```
int G0/0 
ip address 10.0.34.2 255.255.255.252
no shutdown
```
```
int G0/1
ip address 10.0.24.2 255.255.255.252
no shutdown
```
```
int G0/2
ip address 192.168.4.1 255.255.255.0
no shutdown
```

###  configuring  loop back interfaces
1. router1:
```
int l0
ip address 1.1.1.1
```
2. router2:
```
int l0
ip address 2.2.2.2
```
3. router3:
```
int l0
ip address 3.3.3.3
```
4. router4:
```
int l0
ip address 4.4.4.4
```
> to show details about some interface we can use the command
> ```
> do show int l0
> ```

### configuring ospf
1. router1:
```

router ospf 1
network 10.0.12.0 0.0.0.3 0.0.0.3
network 10.0.13.0 0.0.0.3 0.0.0.3
network 1.1.1.1 0.0.0.0 
passive-interface l0
```
2. router2:
```
router ospf 2
network 0.0.0.0 255.255.255.255 area 0
passive-interface l0
```
3. router3:
```
router ospf 3
network 10.0.13.2 0.0.0.0 area 0
network 10.0.34.1 0.0.0.0 area 0
network 3.3.3.3 0.0.0.0 area 0
passive-interface l0
```
4. router4:
```
router ospf 4 
```
> 4 is the ospf process id

```
network 0.0.0.0 255.255.255.255 area 0
```

> **network ipv4-addr wildcard-mask area number**
> 255.255.255.255 wildcard mask is : 0.0.0.0 mask
> 
```
passive-interface g0/0
passive-interface l0
```
> no need to recieve-updates for the local network and recieve routing updates

> loop back interfaces are connected to nothing so no need to recieve-updates

### configure R1 as an ASBR that advertises a default route into the ospf domain
* go router 1 and type 
```
ip route 0.0.0.0 0.0.0.0 203.0.113.2 <!-default route-->
```

```
router ospf 1
default-information originate
```
### check the routing tables of R2 , R3 and R4 what default routes were added.

 if we go to router4 and type:
```
  do show ip route

     1.0.0.0/32 is subnetted, 1 subnets
O       1.1.1.1/32 [110/3] via 10.0.24.1, 00:05:25, GigabitEthernet0/1
     2.0.0.0/32 is subnetted, 1 subnets
O       2.2.2.2/32 [110/2] via 10.0.24.1, 00:06:36, GigabitEthernet0/1
     3.0.0.0/32 is subnetted, 1 subnets
O       3.3.3.3/32 [110/2] via 10.0.34.1, 00:09:37, GigabitEthernet0/0
     4.0.0.0/32 is subnetted, 1 subnets
C       4.4.4.4/32 is directly connected, Loopback0
     10.0.0.0/8 is variably subnetted, 6 subnets, 2 masks
O       10.0.12.0/30 [110/2] via 10.0.24.1, 00:05:35, GigabitEthernet0/1
O       10.0.13.0/30 [110/2] via 10.0.34.1, 00:09:37, GigabitEthernet0/0
C       10.0.24.0/30 is directly connected, GigabitEthernet0/1
L       10.0.24.2/32 is directly connected, GigabitEthernet0/1
C       10.0.34.0/30 is directly connected, GigabitEthernet0/0
L       10.0.34.2/32 is directly connected, GigabitEthernet0/0
     192.168.4.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.4.0/24 is directly connected, GigabitEthernet0/2
L       192.168.4.1/32 is directly connected, GigabitEthernet0/2
O       203.0.113.0/30 [110/3] via 10.0.24.1, 00:02:02, GigabitEthernet0/1
O*E2 0.0.0.0/0 [110/1] via 10.0.24.1, 00:01:55, GigabitEthernet0/1
```
> the routes to all the networks are present on the routing table 
so ospf worked

>O*E2 // is R1's default route

> if we go to r1 and type: 
```
do show ip protocols
```
>we will get
```
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 1.1.1.1
  It is an autonomous system boundary router 
   //**asbr: the access point of the autonomous system**
  Redistributing External Routes from,
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    1.1.1.1 0.0.0.0 area 0
    10.0.13.0 0.0.0.3 area 0
    10.0.12.0 0.0.0.3 area 0
  Passive Interface(s): 
    Loopback0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    1.1.1.1              110      00:05:37
    2.2.2.2              110      00:20:56
    3.3.3.3              110      00:22:41
    4.4.4.4              110      00:19:40
  Distance: (default is 110)

  ```




