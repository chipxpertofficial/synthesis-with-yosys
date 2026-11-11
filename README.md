# Logic Synthesis with Yosys

A hands-on guide to turning Verilog RTL into a gate-level netlist with the
open-source **Yosys** synthesiser — what each step does, how to read the result,
and how it feeds the open RTL-to-GDSII flow.

## Docs
| Doc | Topic |
|-----|-------|
| [00 · Setup](docs/00-setup.md) | installing Yosys, a first `read_verilog` |
| [01 · The synthesis script](docs/01-flow.md) | hierarchy → proc → opt → techmap → abc |
| [02 · Reading results](docs/02-results.md) | cell count, stat, what to check |

## In one paragraph
Synthesis **elaborates** your RTL, lowers behavioural constructs to a generic
gate graph, **optimises** it, then **maps** it to a real standard-cell library
with technology mapping and ABC. Yosys does all of this with a short, readable
script — and the netlist it emits is the input to place-and-route in
[rtl-to-gdsii-flow](https://github.com/chipxpertofficial/rtl-to-gdsii-flow).

## Companion material
- RTL to feed it → [vlsi-verilog-examples](https://github.com/chipxpertofficial/vlsi-verilog-examples)
- The full open PnR flow → [rtl-to-gdsii-flow](https://github.com/chipxpertofficial/rtl-to-gdsii-flow)
- Mentored training → [ChipXpert](https://chipxpert.in/)

## License
MIT — see [LICENSE](LICENSE). Maintained by the ChipXpert team.
