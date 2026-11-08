# RTL-to-GDSII Flow (open tools)

A documented, reproducible walkthrough of the **digital RTL-to-GDSII flow using only free tools**.
It takes a small Verilog design all the way from source to a layout you can view in KLayout, and
explains *what each stage does and why* — the part most flow tutorials skip.

This is a learning flow. It is deliberately small and legible: one simple design, the open
Sky130 PDK, and the OpenROAD/OpenLane tool stack, with every stage's purpose spelled out.

## The stages

A digital implementation flow is the same handful of steps everywhere, open or commercial:

```
RTL (Verilog)
  → Synthesis            (Yosys)        logic → gates mapped to a standard-cell library
  → Floorplan            (OpenROAD)     die/core area, rows, I/O, power grid
  → Placement            (OpenROAD)     put the cells down, legalise
  → Clock Tree Synthesis (OpenROAD)     build a balanced clock network (skew control)
  → Routing              (OpenROAD)     connect everything, meet DRC
  → Signoff              (OpenSTA,      timing (STA), DRC/LVS (KLayout/Magic)
                          KLayout)
  → GDSII                               the layout handed to a foundry
```

Read them in order:

1. [00 · Setup & tool install](docs/00-setup.md) — getting OpenLane/OpenROAD + Sky130 running (the usual blocker).
2. [01 · Synthesis](docs/01-synthesis.md) — Yosys, the standard-cell library, and reading a synthesis report.
3. [02 · Floorplan & power](docs/02-floorplan.md) — utilization, aspect ratio, the power grid.
4. [03 · Placement](docs/03-placement.md) — global vs detailed placement, legalisation, congestion.
5. [04 · Clock Tree Synthesis](docs/04-cts.md) — why skew matters and how CTS controls it.
6. [05 · Routing](docs/05-routing.md) — global/detailed routing, DRC, antenna fixes.
7. [06 · Signoff & GDSII](docs/06-signoff.md) — STA, DRC/LVS, and what GDSII actually is.

## What you need

- Linux (or WSL2 / a Linux VM / Docker — see setup).
- [OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) (which pulls OpenROAD + Yosys + Magic + KLayout via a container), and the [Sky130 PDK](https://github.com/google/skywater-pdk).
- A small synthesizable Verilog design. Start with the counter in [docs/00-setup.md], or bring one
  from [vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples).

## Why open tools for learning

You can run the *entire* flow, inspect every intermediate, and even tape out on a real open shuttle
(Sky130 / Caravel) — with zero licence cost. The concepts transfer directly to commercial tools;
only the command names change.

## Where this fits

- Fundamentals first → [digital-electronics-reference](https://github.com/chipxpertofficial/digital-electronics-reference)
- A block to push through the flow → [vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples)
- A research angle (PPA characterization across the flow) → [mtech-vlsi-projects #08](https://github.com/chipxpertofficial/mtech-vlsi-projects)
- Guided, lab-based PD training → [ChipXpert](https://chipxpert.in/)

## License

MIT — see [LICENSE](LICENSE). Maintained by the ChipXpert team.
