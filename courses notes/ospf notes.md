# OSPF NOTES 
#### ospf versions:
1. ospf version1: old not used anymore
2. ospf version2: used for ipv4
3. ospf version3: used ipv4 and ipv6
#### main terms && process
1. **LSAs**: link state adverstisements
2. **LSDB**: link state database
3. routers will **flood** LSAs until all routers  in the ospf area develop the same map of the network( LSDB)
4. **LSA** example : from router4 {RID:"4.4.4.4" ,ip:"192.168.4.0/24"  , cost:1 } 
> advertising the network 192.168.4.0/24 which is directly connected to R4
> **LSDB** contains a bunch of LSAs
5. using the LSDB each router calculates the shortest path to each router in the network using djkistra algorithm
6. **internal routers** : routers in the same ospf area
7. **backbone routers** : routers which belongs to area 0 (the backbone area which is connected to all the other areas)
8. **ABRS**: area border routers : routers which belongs to the backbone area   another areas (recommended one other area at most)
#### ospf area rules
1. areas should be contigeous 
2. at least one **ABR** on each area connected to the backbone area
3. ospf interfaces in the same subnet must be in the same area.

#### ospf basic configuration
> in R1(config)
```
router ospf <ospf_process_id>
R1(config-router)# network <network-ip-address> <wildcard-mask> area <area-number>

```
> the network command tells the router : to activate ospf on the interfaces with the specified range 
> then the router will try to become neighbor with other ospf-activated nighbor routers
```
passive-interface g2/0
```
>the command tells the router to stop sending OSPF 'hello' messages out of the interface => use it on interfaces which do not have ospf neighbors (local network ,...)
>hover , the router will continue to send **LSA**'s informing it's neighbors about the subnet configured on the interface.

```
default-information originate
```
>if r1 for example have a default route 0.0.0.0/0 to the internet the command will make all the routers in the ospf area
> add a deafult route to the interface  leading to r1 => the default route (usefull to define the entry point for an autonomous system via **ASBR** : autonomous system boundary router )

```
ip protocols
```
> shows usefull informations about the routing protocols :
> protocol name , routerId , routing for : networks ranges , passive interfaces , wither its an **ASBR**,  routing information sources:

|Gateway | Distance | Last update |
|--------|----------|-------------|
|4.4.4.4 | 110      |00:00:08     |
|2.2.2.2 | 110      |00:10:07     |

#### router id order of priority
1. manual configuration
```
router ospf <process-id>
router-id <id as ip address>
clear ip ospf process  //resetting ospf in router to apply changes
```
2. highest ip address on a loopback interface
3. highest ip address on a physical interface
#### changing the default protocal administrative distance 
```
(config-router) distance <distance>
```