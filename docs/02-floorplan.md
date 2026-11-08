# 02 · Floorplanning & power

**Tool:** OpenROAD · **Input:** gate netlist · **Output:** a die/core with rows, I/O, and a power grid

## What it does
Floorplanning decides the **physical canvas**: how big the die and core are, where the standard-cell
rows go, where I/O pins sit, and how power (VDD/VSS) is distributed.

## Key decisions
- **Core utilization** (`FP_CORE_UTIL`) — the fraction of core area filled by cells. 40–60% is
   comfortable for a learning design; push higher and routing gets hard.
- **Aspect ratio** — square is a safe default.
- **Core margin** — space between core and die edge for the power ring.

## The power grid (PDN)
A mesh of VDD/VSS straps on upper metal layers, with rails on the cell rows. Every cell taps power
from this grid. If the PDN is wrong, nothing downstream matters — cells can't get power.

## What to check
- Did the floorplan fit? (utilization not absurd for the cell count).
- Are I/O pins placed sensibly around the edge?
- Is the PDN connected (no floating rails)?

## Why it matters
Utilization is the master trade-off: **too low** wastes area; **too high** causes routing
congestion and DRC failures downstream. Much of "closing" a design is tuning this.

Next: [placement](03-placement.md).
