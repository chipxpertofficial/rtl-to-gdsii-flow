# 04 · Clock Tree Synthesis (CTS)

**Tool:** OpenROAD · **Input:** placed design · **Output:** a balanced physical clock network

## What it does
Until now the clock was treated as "ideal" — reaching every flip-flop at the same instant. It
doesn't. CTS builds a **real buffered clock network** so the clock arrives at all flops with
minimal **skew** (difference in arrival times) and controlled **insertion delay** (total latency
from the clock source).

## Why skew matters
Setup and hold are checked between a launching flop and a capturing flop. If the clock reaches them
at very different times (large skew), you can create hold violations (clock too late at the
capture flop) or eat into the setup budget. CTS keeps skew small and predictable.

## How it's built
A tree of clock buffers (often an H-tree-like structure) driving balanced branches, so every leaf
flop sees a similar delay. The tool inserts and sizes these buffers automatically.

## What to check
- **Clock skew** after CTS (small, within target).
- **Insertion delay** (reasonable, not ballooning).
- **Clock power** — the clock network is often a big chunk of total dynamic power.

## Trade-off
More buffering → lower skew but more clock power and area. This exact trade-off is the subject of
the CTS research template in
[mtech-vlsi-projects #04](https://github.com/chipxpertofficial/mtech-vlsi-projects).

Next: [routing](05-routing.md).
