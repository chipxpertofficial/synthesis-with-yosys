# 00 · Setup

## Install
- **Linux:** `sudo apt install yosys`, or build from source for the latest.
- **With OSS CAD Suite:** the easiest route — one tarball with Yosys, ABC,
  nextpnr, OpenROAD and friends already built. Add its `bin/` to `PATH`.
- **macOS:** `brew install yosys`.

Check: `yosys -V` should print a version.

## A first run
Create `counter.v`:
```verilog
module counter (input clk, rst, output reg [3:0] q);
  always @(posedge clk)
    if (rst) q <= 0; else q <= q + 1;
endmodule
```
Run Yosys interactively:
```
yosys
yosys> read_verilog counter.v
yosys> hierarchy -top counter
yosys> proc; opt
yosys> stat
```
`stat` prints the cell count. You've just elaborated and optimised RTL into a
generic gate graph — no technology yet. Next: a real [synthesis script](01-flow.md).
