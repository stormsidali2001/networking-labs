# Ospf Lab1
### Configuration Steps

* [x] setting up the network structure
  * [x] enable routers interfaces
  * [x] assigning ip addresses to all the interfaces
* [ ] configuring a loop back interface on each router 1.1.1.1/32 for R1 2.2.2.2 for R2 etc...
* [ ] configure ospf on each router (including loopback interfaces)  (do not enable ospf on router1 internet link)
* [ ] configure passive interfaces when appropriate including loopback interfaces.
* [ ] configure R1 as an ASBR that advertises a default route into the ospf domain
* [ ] check the routing tables of R2 , R3 and R4 what default routes were added.

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