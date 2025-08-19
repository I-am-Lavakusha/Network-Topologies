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
