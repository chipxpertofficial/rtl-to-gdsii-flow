# 06 · Signoff & GDSII

**Tools:** OpenSTA, KLayout/Magic · **Output:** verified timing, clean DRC/LVS, and a GDSII file

## What "signoff" means
Signoff is the final set of checks that say the design is **correct and manufacturable**. Three
pillars:

### 1. Static Timing Analysis (STA)
OpenSTA checks every timing path against setup and hold using the post-route delays. You want:
- **Setup slack ≥ 0** — the design meets your target frequency.
- **Hold slack ≥ 0** — no data races faster than the clock.
STA is exhaustive (all paths, no vectors needed) — this is why digital design scales.

### 2. DRC (Design Rule Check)
Confirms the layout obeys every manufacturing rule (via KLayout/Magic rule decks). **Zero
violations** is the bar.

### 3. LVS (Layout Versus Schematic)
Confirms the layout's connectivity matches the gate netlist — i.e. you built *the circuit you
designed*, not a DRC-clean but wrong layout.

## GDSII — what it actually is
GDSII (or the newer OASIS) is the **layered-polygon database** describing every shape on every mask
layer. It is the file a foundry turns into photomasks. Open `results/final/gds/<design>.gds` in
KLayout to see your actual chip.

## The signoff checklist
- [ ] STA: setup and hold slack ≥ 0 across corners
- [ ] DRC: 0 violations
- [ ] LVS: clean (layout matches netlist)
- [ ] Antenna: clean
- [ ] GDSII generated and viewable

Only when all pass is the design "tapeout-ready." On an open shuttle (e.g. Sky130 via Caravel /
Efabless), this GDSII is literally what gets fabricated.

## Where to go next
- Characterize PPA across clock targets and utilization → [mtech-vlsi-projects #08](https://github.com/chipxpertofficial/mtech-vlsi-projects).
- Bring a bigger design through the same flow → [vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples).
