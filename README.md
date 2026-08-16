# The Algorithm Chart, Decoded

An interactive map of the **Korg Volca FM**'s 32 FM algorithms.

### ▶ [Open the interactive version](https://adayeby.github.io/volca-fm-synth-algorithm/)

Korg ships the 32 algorithms as a single dense reference sheet. It tells you how each one is
wired, but not what any of them *sounds* like, or which to reach for when you want a bell
instead of an organ. This page answers both.

Click any of the 32 routings to get:

- **The wiring, drawn large** — signal flow animates down the wires from modulators into
  carriers, with feedback loops traced in orange.
- **Three stats at a glance** — how many operators you actually hear (carriers), how deep the
  modulation chain stacks, and which operator carries feedback.
- **What it's good for** — one line placing the routing between deep FM and pure additive:
  bells and DX bite at one end, drawbar organ at the other.
- **The sound** — the page synthesizes each algorithm live with the Web Audio API, wiring real
  oscillators exactly as the diagram shows. Click through the grid and you can hear algorithm 1
  turn clangorous and algorithm 32 flatten into a plain organ chord.

## Reading a diagram

| | |
|---|---|
| 🟩 **Green** | Carrier — an operator you hear directly. Every green box sums into the output. |
| 🟦 **Blue** | Modulator — not heard directly. It bends the pitch of the box beneath it, which is what creates harmonics. |
| 🟠 **Orange loop** | Feedback — an operator modulating itself, adding a noisy, inharmonic edge. |

Operators stack vertically: each box modulates the one directly below it. More carriers means a
more additive, organ-like sound; longer chains mean denser, more metallic FM.

## Running it

`index.html` is a single self-contained file — no build step, no dependencies, no network
requests. Open it directly in a browser, or serve the folder:

```sh
python3 -m http.server 8000
```

Audio needs a click to start (browsers require a user gesture before playing sound), which the
grid provides on the first algorithm you select.

## Notes

The routing data is the standard DX7 algorithm set, which the Volca FM implements and which its
patches are SysEx-compatible with. Diagrams are generated from that data at runtime rather than
drawn by hand, so each one is derived from the same operator graph the audio engine plays.

Feedback is real, not just drawn: the looped operator runs as a small inline AudioWorklet that
folds each output sample back into its own phase — per-sample, the way the DX chips do it. The
two loop-style algorithms (4 and 6) route through a one-render-quantum delay instead, the
closest a Web Audio graph cycle allows, as phase modulation so the pitch stays put. Feedback
depth is a fixed constant in the code like every other patch parameter: routing is the only
variable between the 32 sounds. Where AudioWorklet isn't available the page plays without
feedback, as before.
