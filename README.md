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

Photo credit @Jeremy's IT lab

## Well designed network (with redundancy)

In this topology, however, there are redundant links even if one fails, frames can still be sent by choosing another path.

<div>
  <img width="529" height="250" alt="image" src="https://github.com/user-attachments/assets/f779a2a0-f99a-4a73-95e4-7d3bc39bd72a" />
</div>

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
  
## Root bridge election
- The lowest root ID bridge will become the Root bridge (By default the Mac-address is used as the tie-breaker.)
- All the root bridges ports will be in Forwarding state
- Other switches in the topology must have a path to reach the root bridge.

<div>
  <img width="460" height="170" alt="image" src="https://github.com/user-attachments/assets/3120ea28-27a3-48ed-b342-eb032afd138f" />
</div>




