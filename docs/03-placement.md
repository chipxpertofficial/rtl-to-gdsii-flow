# 03 · Placement

**Tool:** OpenROAD · **Input:** floorplan + netlist · **Output:** every cell at a legal location

## What it does
Placement decides **where each standard cell sits** inside the core rows, trying to put connected
cells near each other so wires are short (short wires = less delay and less congestion).

## Two phases
1. **Global placement** — spreads cells to minimise total wirelength; cells may overlap slightly.
2. **Detailed placement / legalisation** — snaps cells onto legal row/site positions with no
   overlaps.

## What influences quality
- Floorplan utilization (from the previous stage).
- Timing-driven placement — the tool pulls timing-critical cells closer together.
- Congestion estimation — avoids packing a region so tightly routing can't finish.

## What to check
- **Congestion map** — red/hot regions predict routing trouble. If it's bad, lower utilization and
   re-run.
- **Post-placement timing** — now that cells have locations, wire delays are estimated; slack gets
   more realistic than after synthesis.

## Common pitfalls
- High utilization + a congested macro region → placement "succeeds" but routing fails later.
- Ignoring the congestion report until routing blows up.

Next: [clock tree synthesis](04-cts.md).
