# 00 · Setup & tool install

The single biggest barrier to the open flow is getting the tools installed. OpenLane solves this
by shipping everything in a container, so you don't build Yosys/OpenROAD/Magic by hand.

## Prerequisites
- Linux, or WSL2 on Windows, or a Linux VM. macOS works via Docker too.
- Docker installed and working (`docker run hello-world` succeeds).
- `git`, `make`, and ~10 GB free disk.

## Install OpenLane + Sky130
```sh
git clone https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane
make            # pulls the tool container and builds the Sky130 PDK
make test       # runs the built-in SPM design end-to-end — your install smoke test
```
If `make test` produces a GDS for the `spm` design, your flow works. That is the goal of this page.

## A minimal design to start with
Create `designs/counter/src/counter.v`:
```verilog
module counter #(parameter W = 8) (
    input  wire         clk,
    input  wire         rst_n,
    input  wire         en,
    output reg  [W-1:0] count
);
    always @(posedge clk or negedge rst_n)
        if (!rst_n)      count <= '0;
        else if (en)     count <= count + 1'b1;
endmodule
```

And a minimal `designs/counter/config.json`:
```json
{
  "DESIGN_NAME": "counter",
  "VERILOG_FILES": "dir::src/*.v",
  "CLOCK_PORT": "clk",
  "CLOCK_PERIOD": 10.0,
  "FP_CORE_UTIL": 40
}
```

## Run the flow
```sh
./flow.tcl -design counter
```
When it finishes, the results land under `designs/counter/runs/<timestamp>/results/`. The final
layout is `final/gds/counter.gds` — open it in KLayout.

## What to look at first
- `reports/synthesis/` — gate count after synthesis.
- `reports/signoff/` — timing (STA) and DRC summaries.
- `results/final/gds/counter.gds` — the layout.

Each of the following pages explains the stage that produced these.

## Common install snags
- Docker permission denied → add your user to the `docker` group and re-login.
- Slow first run → the PDK build and container pull are one-time; later runs are fast.
- WSL2 disk → keep the repo on the Linux filesystem, not `/mnt/c`, or it will crawl.
