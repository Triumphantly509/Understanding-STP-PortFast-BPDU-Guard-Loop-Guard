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

##Without STP

- Broadcast storms – broadcast frames circulate endlessly.
- MAC address table instability – switches constantly relearn MAC addresses on different ports.
- Multiple frame copies – the same frame is received multiple times.

## How does STP work

Let's take this topology as an example:

<div>
  
</div>


