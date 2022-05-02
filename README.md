# 1. Introduction
RIPV2 is a networking protocol that allows the router to know each other its Distance Vector protocol means that the 
routers will manipulate tables of hop number to other router here a victor is the table and the distance is the number
of hops these protocols apply algorithms like Bellman-Ford to do the job.

Before RIPV2 we had RIP(V1 the V stands for the version) the main difference between both of them is that RIPV2 is classless
and RIP is classfull means that RIP doesn't include the subnet mask with network address in the routing update

# 2. Configure the ip address of the machines 
In this case it's so easy go ahead click on the computer and add the ip address

# 3. Configure the ip address of the Routers Interfaces
Same thing don't waste your time, click on the router and add ip addresses as the schema show in the png file 

# 4. Configuring RIPV2 on the Routers

## R1 and R2
```
router rip
 version 2
 passive-interface GigabitEthernet0/0
 passive-interface GigabitEthernet0/1
 passive-interface GigabitEthernet0/2
 network 10.0.0.0 // computer network address
 network 192.168.0.0 // routers network address
 no auto-summary
```
## R4
for this one you just have to add the ip addresses on the right interface.

## R3

```
router rip
 version 2
 passive-interface GigabitEthernet0/0
 network 192.168.0.0
 default-information originate
 no auto-summary
```
Configuring default-information originate on the middle router’s RIP process tells RIP to distribute a
default route out the interfaces on which it is enabled to all other RIP-speaking routers.

from now, we are not done yet, but we are almost there as I said we have to distribute a kind, of a catch-all route a 
route that we are going to use when we don't dunno where to go like for example going to the internet. and this route is
the default route and this is how you add it.
```
ip route 0.0.0.0 0.0.0.0 <next-hop>  in our case -> 200.10.1.18  
```
# NAT Configuration
NAT enables private IP internet works that use Unregistered IP addresses to connect to the Internet.
NAT operates on a device, usually connecting two networks. Before packets are forwarded onto another network, 
NAT translates the private (not globally unique) addresses in the internal network into legal addresses.
NAT can be configured to advertise to the outside world only one address for the entire network. 
This ability provides more security by effectively hiding the entire internal network behind that one address.
-- FROM CISCO

In Our Case to Configure this first you will have to pick your outside in inside interface go the router
and do the following :

1. Inside :
```
interface <Interface-Name>
ip nat inside
```
2. Outside
```
interface <Interface-Name>
ip nat outside
```
Now we are going to use a method that will allow the PAT(Port Address Translation) this will help the machines connect
to the internet using a single public ip address but use the port to translate the response back to the machine that 
requested it 

```
ip nat inside source list 1 interface <OUT-INTERFACE> overload
```



