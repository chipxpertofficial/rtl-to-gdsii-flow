# 05 · Routing

**Tool:** OpenROAD · **Input:** placed + CTS design · **Output:** all nets physically connected, DRC-clean

## What it does
Routing draws the actual **metal wires and vias** that connect cell pins, across the PDK's metal
layers, obeying the design rules (spacing, width, via rules).

## Two phases
1. **Global routing** — assigns each net to coarse routing regions and layers; estimates congestion.
2. **Detailed routing** — lays down exact wires and vias, resolving every design-rule constraint.

## Design Rule Checking (DRC)
The router must finish with **zero DRC violations** — spacing, minimum width, via enclosure, etc.
A route that "completes" with DRC errors is not a valid result; the layout is not manufacturable.

## Antenna effect
During manufacturing, long metal segments connected to a gate can accumulate charge and damage the
gate oxide. The flow fixes this by inserting **antenna diodes** or jogging the route to a higher
layer. OpenROAD handles antenna fixing as part of routing/signoff.

## What to check
- **DRC = 0** after detailed routing.
- **Post-route timing** — now wires are real, this is the truth; earlier numbers were estimates.
- **Congestion** — if routing couldn't finish cleanly, go back and lower utilization.

## Common pitfalls
- High utilization → unroutable regions → DRC violations that no amount of retry fixes.
- Forgetting that post-route timing is what counts; pre-route slack can be optimistic.

Next: [signoff & GDSII](06-signoff.md).
