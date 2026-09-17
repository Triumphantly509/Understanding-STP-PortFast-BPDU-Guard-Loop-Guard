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

## Part two - Each remaining SW in the topology will select one of its interfaces to be Root Port.
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

- On Sw 2, interface Gig1/0/1 is the Root Port, 0 + 4 = 4, (lowest root cost). From interface Gig1/0/2, the root cost is 4 + 4 = 8.
## Switch 2
<div>
  <img width="630" height="309" alt="image" src="https://github.com/user-attachments/assets/f7e5c40b-063a-44cd-aae4-5080326ebfb1" />
</div>

- On switch 3, interface Gig1/0/2 is the root port, because its root cost is 4, however from interface Gig 1/0/1 it is 8.
- Interface Gig 1/01 is blocked, but interface Gig 1/0/2 from Sw 2 is Designated port.
## Switch 3
<div>
  <img width="632" height="307" alt="image" src="https://github.com/user-attachments/assets/30ee2055-e3bf-49e2-8d89-2f7df14d1f23" />
</div>


## Part two - What if a switch has multiple ports with the same Root Cost?
- Lowest neighor bridge ID

## Example
<div>
  
</div>




