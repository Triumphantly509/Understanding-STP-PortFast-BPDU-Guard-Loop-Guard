# Understanding-STP-PortFast-BPDU-Guard-Loop-Guard

## Objective

Learn how STP prevents Layer 2 switching loops by creating a loop-free topology while maintaining network redundancy.

## Network Redundancy examples

## Poorly designed network (With no redundancy)

No redundancy mean, if a link fails. There's no other alternatives, or no other path that can ensure connectivity.
In other words, there won't be internet connection, which is critical for business. Imagine 1h internet issue at Google..

<div>
  <img width="563" height="238" alt="image" src="https://github.com/user-attachments/assets/bad488a1-575a-4f87-89b2-c35e08241f78" />
</div>

## Well designed network (with redundancy)

In this topology, however, there are redundant links even if one fails, frames can still be sent by choosing another path.

<div>
  <img width="529" height="250" alt="image" src="https://github.com/user-attachments/assets/f779a2a0-f99a-4a73-95e4-7d3bc39bd72a" />
</div>

Photos credit @Jeremy's IT lab

##Without STP, there are issues that can destroy your network.

- Broadcast storms – (Due to the flooding of broadcast and unknown unicast frames to the LAN)
  
- MAC address table instability – (switches constantly relearn MAC addresses on different ports.)
  
- Multiple frame copies – (the same frame is received multiple times.)

## This is the reason why that STP is used to prevent loops, set ports on forwarding or blocking ...

## For now let's focus on STP IEEE 802.1D

## How does STP work

Referring to this topology as an example:

<div>
  <img width="394" height="218" alt="image" src="https://github.com/user-attachments/assets/1fe42111-bab8-41d9-8f98-968b066dec34" />
</div>

- I use only switches because, PCs and Routers do not use, STP, Hello BPDUs..., except switches.
-  Any enabled switch share a Hello BPDU to one another every 2 seconds, that's how a switch knows that other switches are still operational.
-  If all the switches were not linked to each other, there won't be any loops, so no STP would be needed.
-  Bridge = Switch
  
## Part one - Root bridge election
- The lowest root ID bridge will become the Root bridge (By default the Mac-address is used as the tie-breaker.)
- All the root bridges ports will be in Forwarding state
- Other switches in the topology must have a path to reach the root bridge.

<div>
  <img width="460" height="170" alt="image" src="https://github.com/user-attachments/assets/3120ea28-27a3-48ed-b342-eb032afd138f" />
</div>

## Cisco switches use a version of STP called PVST (Per-VLAN Spanning Tree) PVST runs a separate STP instance in each VLAN, on each vlan diffrent interfaces can be FW/BLK.
- Bridge ID is an increment of 4096
- The lowest value can be 0
- The valid values: 0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768
- 36864, 40960, 45056, 49152, 53248, 57344, or 61440
- The bridge ID value can be the sum (32768 + the VLAn ID)
- 32768 + 1 (Default Vlan ID) = 32769

## This is how to know the VLAN ID

<div>
  <img width="434" height="143" alt="image" src="https://github.com/user-attachments/assets/810bcd24-d5ff-449a-baeb-3f4e1e8da8e0" />
</div>

- The default VLAN ID is 1.

## Answer these questions, which switch will be the Root bridge.
## Example 1
<div>
  <img width="876" height="452" alt="image" src="https://github.com/user-attachments/assets/3fe96d5f-b838-4390-ba12-302b23942cf5" />
</div>
## Example 2
<div>
  <img width="725" height="465" alt="image" src="https://github.com/user-attachments/assets/23ca210c-6fee-4ce7-bec6-dffbbefa87c0" />
</div>

## Example 3
<div>
  <img width="882" height="469" alt="image" src="https://github.com/user-attachments/assets/9e12ecdd-ae29-45c0-a085-f57995c479f5" />
</div>

## Part two - 1 - Each remaining SW in the topology will select one of its interfaces to be Root Port.
- Interfaces with the lowest root cost is the root port

## Root cost
- 10 Mbps = 100
- 100 Mbps = 19
- 1 Gbps = 4
- 10 Gbps = 2

## Which port will be the Root port on each switch that is not the root bridge?
- Example
<div>
  <img width="968" height="430" alt="image" src="https://github.com/user-attachments/assets/a8f046ef-f4d1-4f9e-b4cf-7408987b33e7" />
</div>

## Result
<div>
  <img width="922" height="424" alt="image" src="https://github.com/user-attachments/assets/a55674f0-9f22-48ab-9e9f-a0b4bf3f17b4" />
</div>

## Visualize the result from each separate switch.

- Switch 1 is the Root bridge, because it has the lowest Mac address. (All the root bridges ports will be in Forwarding state)
  
## Switch 1
<div>
  <img width="723" height="371" alt="image" src="https://github.com/user-attachments/assets/f351dc70-a4d8-467d-aed3-65f20f38e5e2" />
</div>

- On Sw 2, interface Gig1/0/1 is the Root Port, 0 + 4 = 4, (lowest root cost), but from interface Gig1/0/2, the root cost is 4 + 4 = 8, it is just a Designated port.
## Switch 2
<div>
  <img width="630" height="309" alt="image" src="https://github.com/user-attachments/assets/f7e5c40b-063a-44cd-aae4-5080326ebfb1" />
</div>

- On switch 3, interface Gig1/0/2 is the root port, because its root cost is 4, however from interface Gig 1/0/1 it is 8.
- Interface Gig 1/01 is blocked.
## Switch 3
<div>
  <img width="632" height="307" alt="image" src="https://github.com/user-attachments/assets/30ee2055-e3bf-49e2-8d89-2f7df14d1f23" />
</div>


## Part two - 2 - What if a switch has multiple ports with the same Root Cost?
- Lowest root cost
- Lowest neighbor bridge ID / lowest Mac address

## Example
<div>
  <img width="650" height="433" alt="image" src="https://github.com/user-attachments/assets/11e7a5f2-ef0d-474b-969e-b5d7b39b6953" />
</div>

## Result and explanation
<div>
  <img width="673" height="382" alt="image" src="https://github.com/user-attachments/assets/1e103f62-3aa3-4e3d-86c9-c065007c8f5b" />
</div>

- Switch 12 is the root bridge, while all the BID are the same, the tiebreaker is the lowest mac address. As a result, its interfaces are Designated Ports.
  <div>
    <img width="632" height="288" alt="image" src="https://github.com/user-attachments/assets/ff5b8654-fdac-425d-80ed-10b2b7682bd1" />
  </div>

- On switch 14, interface Gig 1/0/1 is the root port because its root cost is just 4.

 <div>
  <img width="632" height="306" alt="image" src="https://github.com/user-attachments/assets/95ecaece-7e86-4131-8749-b233fa0e4300" />
</div>

- On switch 11, interface Gig 1/0/1 is the root port as well, because it also has 4 as root cost.

  <div>
    <img width="631" height="306" alt="image" src="https://github.com/user-attachments/assets/ebdad252-600d-49a3-8863-1053e3e8916b" />
  </div>
  
- On switch 0, however the switch has 2 ports with same root path cost, 8.
- 2 solutions possible: lowest neighbor bridge ID / lowest Mac address.
- Switch 14 and switch 11 have the same bridge ID but switch 14 has the lowest Mac address.
- The best path to the root bridge from SW0 is Gig 1/0/1, sw14, the root bridge.

<div>
  <img width="641" height="308" alt="image" src="https://github.com/user-attachments/assets/59ab23b1-0ec0-4327-b295-194e44a4f9cc" />
</div>

- To decide which port will be blocked or designated between switch 0 and switch 11.
- Ask who has the lower root path cost. switch 11 to reach the root bridge, its root path cost is 4, switch 0 root path cost is 8.
- As a result, Gig 1/0/2 on switch 11 is Designated port, Gig 1/0/2 on switch 0 is blocked by STP.
  
## Part two - 3 - What if 2 switches have 2 connections between them, both Root cost and neighbor bridge ID are the same?

- Lowest neighbor port ID

<div>
  <img width="744" height="365" alt="image" src="https://github.com/user-attachments/assets/36130f0b-dbda-4eee-be8e-3c6cf5cbea0a" />
</div>

## Explanations

<div>
  <img width="744" height="365" alt="image" src="https://github.com/user-attachments/assets/f17d8a7d-f6d7-4674-8896-7b7bbc591c9c" />
</div>

- Sw3 is elected root bridge because it has the lowest Mac address.
- Sw2 via Gig 1/01 is the root port, as well as sw 0 via Gig 1/0/1.
- Sw1 has 2 connections with Sw 0 having the same root cost each.
- To determine the root port on SW 1, the lowest neighbor port Id should be used as the tiebreaker.
- sw 0 is the neighbor, let's verify the port ID values

<div>
  <img width="618" height="301" alt="image" src="https://github.com/user-attachments/assets/6602792d-23bf-4f4f-b5e7-891251ccac4b" />
</div>

- interface Gig1/0/2 on switch 0 has lower value compare to Gig1/0/3
- Therefore, int Gig 1/0/2 on switch 1 is the root port and Gig1/0/2 is the designated port
- to determine which port to block between Gig1/0/3 on switch 1 and Gig1/0/3 on switch 0.
- Ask who has lower root path cost? sw0 has 4, sw 1 has 8, the lowest one is the designated port Gig 1/0/3 on switch 0 and Gig 1/0/3 is blocked.
- Repeat the same process with switch 1 and switch 2. switch 2 has the lowest root path cost, interface Gig1/0/2 is designated port and Gig 1/0/1 on switch 1 is blocked.

  <div>
    <img width="794" height="351" alt="image" src="https://github.com/user-attachments/assets/0f5a406c-22f4-4081-bef8-7223a77c924e" />
  </div>

  ## STP ports state Review
  <div>
    <img width="1295" height="572" alt="image" src="https://github.com/user-attachments/assets/2e50f725-0a2e-47a6-a53d-c8dd07acc2db" />
  </div>

  ## STP Timer
  <div>
    <img width="1183" height="541" alt="image" src="https://github.com/user-attachments/assets/31b72615-9513-4cc9-b12c-415000accdfc" />
  </div>

  - Jeremy's IT lab photo
 
## Port Fast
- Portfast allows a port to move immediately to the Forwarding state, bypassing Listening and Learning.
- If used, it must be enabled only on ports connected to end hosts.
- If enabled on a port connected to another switch it could cause a layer 2 loop.

## Consider this diagram

<div>
  <img width="841" height="404" alt="image" src="https://github.com/user-attachments/assets/fa83ee6d-80e9-4445-a02e-e36ea6f409af" />
</div>

## Enable RPVST on all the switches like this
<div>
  <img width="635" height="138" alt="image" src="https://github.com/user-attachments/assets/34da5b8f-dd2a-41b5-afc0-11dcf09ec222" />
</div>

## Verify which switch is the root bridge
- Switch 2 is the Root bridge
<div>
  <img width="632" height="322" alt="image" src="https://github.com/user-attachments/assets/a9c773d9-33fe-42ed-ac1c-fc2b43506c62" />
</div>

## Let's make switch 1 the root bridge
<div>
  <img width="632" height="193" alt="image" src="https://github.com/user-attachments/assets/84302c3a-9d4a-4d9d-8207-dadea1e88ae3" />
</div>

## Let's configure switch 0 as secondary bridge
<div>
  <img width="630" height="280" alt="image" src="https://github.com/user-attachments/assets/1c16bfc9-dcaf-45c9-83b4-72291db5fa49" />
</div>

## Let's configure switch 2 as secondary bridge as well
<div>
  <img width="621" height="238" alt="image" src="https://github.com/user-attachments/assets/376b9660-4eab-4335-ba5b-8636bbaf80cb" />
</div>

## Enabling PortFast on all access ports (not trunk ports)

## How to verify

## BPDU Guard
- If an interface with BPDU Guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loo from forming.

## How to enable BPDU Guard

## To enable enable a port that was disabled by BPDU Guard

## How to verify

## Root Guard

## Loop Guard


