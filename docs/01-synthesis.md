# 01 · Synthesis

**Tool:** Yosys · **Input:** RTL Verilog + a standard-cell `.lib` · **Output:** a gate-level netlist

## What it does
Synthesis turns your behavioural RTL into a netlist of **real standard cells** from the PDK
library (AND, OR, DFF, buffers, …), optimised for your timing constraint. `count <= count + 1`
becomes an adder built from library gates; an `always` with a clock becomes flip-flops.

## The three sub-steps
1. **Elaboration** — parse RTL, infer registers vs combinational logic.
2. **Optimization** — constant propagation, logic minimisation, FSM encoding.
3. **Technology mapping** — map the optimised logic onto the specific cells in the `.lib`, choosing
   drive strengths to meet timing.

## What drives the result
- **Clock period** (`CLOCK_PERIOD`) — a tighter period makes the tool work harder (bigger, faster
   cells; more area/power).
- **The standard-cell library** — the menu of cells available and their delay/area/power.

## Reading the report
Look for:
- **Cell count / area** — how big the logic is.
- **Chip area estimate** — a first PPA signal (not final; placement/routing change it).
- **Worst slack after synthesis** — is the logic even theoretically fast enough? A negative number
   here means no amount of place-and-route will save you; relax the clock or restructure the RTL.

## Common pitfalls
- **Latches you didn't want** — an incomplete `if`/`case` in combinational logic infers a latch.
   Yosys warns; fix the RTL.
- **Unmapped constructs** — non-synthesizable code (delays, `initial` for logic) won't map.
- **Treating synthesis area as final** — it is an estimate; routing adds real wire load.

Next: the netlist goes to [floorplanning](02-floorplan.md).
