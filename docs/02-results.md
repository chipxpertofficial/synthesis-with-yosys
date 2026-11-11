# 02 · Reading synthesis results

## `stat` — the first thing to check
```
=== my_top ===
   Number of cells:   1240
     $_DFF_P_           128
     NAND2_X1           310
     ...
   Chip area for module: 4821.2
```
Look for:
- **Flop count** matches what you expect from your registers. Way more → you
  inferred latches or accidental registers. Way fewer → logic got optimised away
  (maybe a bug — a dangling output tied off).
- **Area** — your rough size budget.
- **Latches** (`$_DLATCH_`) — almost always a bug: a combinational `always` that
  doesn't assign a signal on every path. Fix the RTL, don't ship the latch.

## `check` and warnings
`check` reports combinational loops, multiple drivers, and undriven wires. Treat
these as errors — they bite in PnR or silicon, not in your quick sim.

## Sanity: equivalence
After optimisation, confirm the netlist still equals the RTL. Yosys can do a
bounded check (`equiv_*` / `sat`), or use a formal LEC tool. Never trust a
netlist you haven't equivalence-checked against the source.

## Handing off
`write_verilog` (or `write_json` for nextpnr) produces the netlist that
place-and-route consumes — the next stage in
[rtl-to-gdsii-flow](https://github.com/chipxpertofficial/rtl-to-gdsii-flow).
Carry the same Liberty library forward so timing numbers stay consistent.
