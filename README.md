Network Topologies – Combined Report

This repository contains study and practice notes on different network topologies using switches, routers, VLANs, static routing, RIP, and multilayer switch (SVI).

1. Layer 2 Switch Topology
- A simple network created with a Layer 2 switch and two hosts.
- Hosts assigned Class A IP addresses.
- Verified IPs using ipconfig.
- ARP cache and switch MAC address table were initially empty (show mac address-table dynamic).
- After ARP broadcast:
  - Switch learned MAC addresses.
  - ARP cache updated on both hosts.
- ICMP Echo Request/Reply (ping) confirmed connectivity.

2. Router Topology (Single Router for Two Networks)
- Two networks: 10.0.0.0 and 11.0.0.0, each with its own switch and hosts.
- A single router connected both networks and acted as default gateway
  - 10.0.0.1 for 10.0.0.0 network.
  - 11.0.0.1 for 11.0.0.0 network.
- Initially, ARP caches and MAC tables were empty.
- Process:
  - Hosts sent ARP requests to learn gateway MAC.
  - Router then performed ARP in the other network.
  - ICMP request/reply completed successfully.

3. Static Routing & RIP (Two Routers)
- Topology: two networks connected via two routers and switches.
- Each router provided gateway IPs to its connected network.
- Static Routing:
  - Required manual route configuration.
  - Example:  
    ip route 11.0.0.0 255.0.0.0 1.0.0.2
  - Both routers had to be configured.
  - TTL reduced by 2 since two hops existed.
- RIP (Routing Information Protocol):
  - Automates route learning using hop count.
  - Routes marked with `R` in routing table.
  - Config example:
    router rip
    version 2
    network 12.0.0.0
    network 13.0.0.0

4. VLAN Topology
- A switch with 5 devices grouped into VLANs: 100, 200, 300.
- Devices within the same VLAN communicated successfully:
  - ARP requests/responses exchanged.
  - ICMP Echo Request/Reply completed.
- Communication across VLANs failed:
  - Switch only forwards broadcasts within a VLAN.
  - Devices in VLAN 200 cannot reach VLAN 100 without inter-VLAN routing.

5. Multilayer Switch (SVI – Inter-VLAN Routing)
- VLANs created: 100, 200, 300.
- Configured Switch Virtual Interfaces (SVIs) for inter-VLAN routing:
  - VLAN 100 → 192.168.1.1/26
  - VLAN 200 → 192.168.1.65/26
  - VLAN 300 → 192.168.1.129/26
Example:
vlan 100
interface vlan 100
ip address 192.168.1.1 255.255.255.192
no shutdown

We should use the command ip routing before confugring this then only these confugurations will be applied.
Without router we can achieve the inter vlan routing in this multilayer switch or layer 3 switch.



There are other protocols in networking layer 2 and 3 which are yet to be explored.

Today I have created the topology which enabled me to do the multiple networking configurations together.

Created the OSPF topology in Cisco Packet Tracer using Areas 0, 1, and 2.
Configured inter-router IP addressing
Configured VLANs 10, 20, 30, 40, 50, and 60 in Layer 3 switches and routers.
Configured DHCP server with dhcp pools for all VLANs.
Verified OSPF adjacencies, LSAs, and routing tables using commands like:
Show ip ospf neighbor
Show ip route ospf
Show ip ospf interface brief

The problems that I have faced are

OSPF area mismatch errors

Verified all backbone links were in Area 0 and corrected the area configuration. After fixing, OSPF adjacency came up successfully

Missing adjacency between R1–L3 Switch and R3–L3 Switch

Corrected subnet masks and added proper network statements for inter-router links and VLANs. Verified adjacency using 

show ip ospf neighbor
PCs were not receiving IP addresses

Forgot to add ip helper address on R2 subinterfaces for VLAN50 and VLAN60.

Configured ip helper address under multiple routers. PCs in both VLANs then obtained  successfully



After this corrections

All routers learned inter-area routes

192.168.10.0/24, 192.168.20.0/24 (Area 1)

192.168.30.0/24, 192.168.40.0/24 (Area 2)

192.168.50.0/24, 192.168.60.0/24 (Area 0)

All VLANs received dynamic IPs from the DHCP server.

DHCP relay tested via simulation mode  Discover → Offer → Request → ACK completed successfully.

End-to-end connectivity verified between PCs in VLAN10, VLAN30, and VLAN60.





The core commands that I have learnt today are

Show ip ospf neighbour

To check the neighbour routers
Show ip route ospf 

To see the routes that are learnt by ospf
Show ip interface brief

To sse the details about the interfaces and their state
Show ip dhcp binding

To see the devices that got ip and their mac and interfaces.


Finally Understood the hierarchical design of OSPF and the role of the backbone.

Gained practical experience in configuring DHCP relay agents for inter-area clients.

Learned structured troubleshooting for layer 2 and layer 3 connectivity issues


