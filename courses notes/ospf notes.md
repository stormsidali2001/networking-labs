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

#### OSPF metric (cost)
1. its calculated as follow :  **reference bandwith** / **interface bandwith**
> fast ethernet 10 mbps : const = 100 mbps / 10mbs = 10
> fast ethernet 100 mbps : const = 100 mbps / 100mbs = 1
> gigabit ethernet 1000 mbps : const = 100 mbps / 1000mbs = 1
> gigabit ethernet 10000 mbps : const = 100 mbps / 10000mbs = 1
2. changing the **reference bandwith** 
```
auto-cost reference-bandwith <megabit-per-seconds>
```
3. displaying an interface ospf cost
```
ip ospf interface <interface-name>
```
4. example suppose that we 've configured the **reference bandwith** in all routers to be 100000 mbps
and have a path r1(g0/0)->(g0/0)r2(g1/0)->(g0/0)r4(g1/0)->192.168.4.0/24 network
whats the cost of the path ? 
>answer: the same of all **exiting** interfaces of all the routers in the path
> cost(r1,g0/0) +cost(r2,g1/0) +cost(r4,g1/0)  = 100000/1000 + 100000/1000 + 100000/1000 = 100 + 100 + 100 = 300
5. what about loopback interfaces : whats r1's cost to reach r2's loopback 0 interface
> cost(r1,g0/0) + cost(r2,l0) = 100 + 1 = 101 
> the cost of a loopback interface is : 1
6. we can set a manual value to an interface ospf cost (takes priority over the auto calculated cost)
```
(config-if)# ip ospf cost 10000
```
7. to change the interface bandwith (not recommeded , do not affect the speed of the interface):
```
bandwith <bandwith in kilobits>
```
8. use the following command to display an overview on each ospf interface configured in the router (cost ,ip addr , state, neighbors,area)
```
show ip ospf brief
```
#### OSPF Neighbors
1. once routers become neighbors they automatically do the work of sharing network information , calculating routes ...
2.  when ospf is activated on an interface , the router starts sending ospf ***hello messages** out of the inerface at regular intervals (determined by the hello timer , they are used to introduce the router to potentiel neighbors)
3.  the default hello timer is 10 seconds on an ethernet connection
4.  hello messages are multicast to 224.0.0.5 (multicast address for all ospf routers).
5.  ospf messages are encapsulated in the ip header ,with a value of 89 in the protocol field 