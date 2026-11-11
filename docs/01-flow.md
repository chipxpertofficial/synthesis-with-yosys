# 01 · The synthesis script, step by step

A typical ASIC synthesis script for a Liberty library:
```tcl
read_verilog design.v
hierarchy -top my_top          ; # resolve the module tree, pick the top
proc                           ; # always-blocks -> netlist of muxes/registers
flatten                        ; # (optional) remove hierarchy for cross-opt
opt                            ; # constant propagation, dead logic removal
fsm                            ; # detect & optimise state machines
memory                         ; # infer/May map RAMs
techmap                        ; # lower to generic gate primitives
dfflibmap -liberty lib.lib     ; # map flops to the library's flops
abc -liberty lib.lib           ; # map combinational logic + optimise (ABC)
opt_clean                      ; # remove now-unused wires/cells
write_verilog netlist.v        ; # gate-level netlist out
stat -liberty lib.lib          ; # area/cell report
```
The one-liner `synth -top my_top` runs the generic version of most of this; the
explicit script is what you use when mapping to a real library.

## What the key steps actually do
- **proc** — converts procedural RTL (`always`, `if`, `case`) into a graph of
  multiplexers and registers. Nothing is a "gate" yet.
- **opt** — the cleanup workhorse; run it between heavy steps.
- **techmap** — rewrites high-level cells (adders, shifters) into Boolean
  primitives ABC can optimise.
- **dfflibmap** — picks the right flavour of flop (with/without reset, enable)
  from your library.
- **abc** — the real logic optimiser/mapper: minimises the combinational logic
  and maps it to library gates, optionally timing-driven (`abc -liberty ... -D`).

## FPGA targets
Swap the back end: `synth_ice40 -json out.json`, `synth_ecp5`, `synth_xilinx`.
The front half (proc/opt/fsm/memory) is the same; only mapping differs. See
[fpga-build-guides](https://github.com/chipxpertofficial/fpga-build-guides).
