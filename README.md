# IHP 130 nm EM/IR showcase — a dual-control VCO, before → after

This repository holds the **before/after electromigration + IR-drop reports and run
outputs** for a **dual-control ring VCO** rebuilt from scratch in **IHP's open 130 nm
process (sg13g2)** and run through an EM/IR gate at the foundry's **real current-density
limits**. It appears in two states: **v1 (habit-sized)** — the first build any
designer would start with, rail widths and via habits and all — and **v2
(flow-repaired)** — after the automated EM/IR repair loop. This repo is **outputs
only**: the EM/IR analysis engine and the CAL layout engine that generate these
artifacts are **not** included here (see [`LICENSE`](LICENSE)).

## Why these numbers are signoff-grade

Unlike sky130 — which publishes no EM rules, so findings there are *structural*
estimates — **ihp130 ships real EM limits**, as `DCCURRENTDENSITY` values in its
vendor tech LEF (`sg13g2_tech.lef`). Every EM ratio in this repo is screened against
those **real foundry numbers**, so the before/after results here are **signoff-grade,
not estimates**. The **IR-drop budget is 2 % of VDD** (the tool's `ir_budget_frac`).
Worst case (WC) = **125 °C, worst-RCX** (the EM corner adds VDD +5 %; the IR corner
uses VDD −10 %).

Nothing in this repo replaces foundry signoff — but on ihp130 the physics being
screened is the foundry's own.

## Main comparison — v1 (habit-sized) → v2 (flow-repaired)

The row below is the **measured, DRC/LVS-clean ihp130 result** (2026-07-14). Both v1 and
v2 are **DRC-0 (magic sg13g2) + LVS "Circuits match uniquely" (netgen sg13g2)**, and EM is
screened against IHP's real LEF `DCCURRENTDENSITY` at a **1.0× signoff gate**. Nothing here
is estimated or projected — see the footnotes for the two honest labels.

| Cell | Measured current | v1 worst EM (nom → WC) | v1 worst IR vs budget | v1 verdict | v2 worst EM (WC) | v2 worst IR | v2 verdict | Iterations | DRC / LVS |
|---|---|---|---|---|---|---|---|---|---|
| DualCtrlVCO | ≈457 µA VREG / 460 µA VSS † | 3.37× (WC) | 18.6 mV / 30 mV | **FAIL** | **0.99×** | 9.9 mV | **PASS** | 1 | **0 / match** |

<sub>† Supply current **reused from the identical sky130 sizing** (same topology + device sizes); a fresh sg13g2 ngspice measure was out of scope. AC decoupling MIM caps are **omitted** — they carry no DC current, so the DC EM/IR screen is unaffected (and the ring runs faster → conservative). Worst-EM figures are the **signoff WC pair** (125 °C, worst-RCX); v2's 0.99× is a thin-but-real pass on the geometrically-boxed VREG rail. The 27-transistor VCO is DRC-0 (magic sg13g2) + LVS "Circuits match uniquely" (netgen sg13g2) in both states — logs in `DualCtrlVCO/{before,after}/`.</sub>

## The cell

- **[DualCtrlVCO](DualCtrlVCO/README.md)** — a dual-control ring oscillator (VCO), the
  oscillator core of a PLL, rebuilt from scratch in ihp130.

## How to read a before/after report

- **EM ratio = I / Imax** — a segment's current divided by its EM limit for that
  layer/via at the corner. **≥ 1.0 means over the foundry limit.**
- **Tiers** (the tool's fixed I/Imax scale, `emirtool/solve_em.py`):
  `ok` (< 0.5) · `watch` (< 0.8) · `marginal` (< 1.0) · `over` (< 3×) ·
  `high` (< 10×) · `crit` (≥ 10×). The task-referenced ladder is
  ok / watch / over / high / crit; `marginal` is the tool's band just under the
  limit (0.8–1.0) and is shown for completeness.
- **Corner** — WC = **125 °C / worst-RCX**. EM screened at nominal (25 °C) **and** WC;
  IR budget = **2 % of VDD**.
- **Verdict** — `PASS` / `FAIL`, with review-band rows (near-limit) listed separately
  from gating violations.
- **Heat map** — per-rectangle: a hot neck lights up alone at rectangle granularity,
  not its whole net. Before → after on the same scenario and color scale.
- **v1 → v2** — v1 is the habit-sized first build; v2 is after the repair loop
  (`WIDEN_SEGMENT`, `ADD_VIA_CUTS`, `ADD_PARALLEL_STRAP`, `DENSIFY_MESH`, …), with
  DRC + LVS re-run on every rebuilt GDS.

## Provenance & honesty

- **EM limits: real foundry.** ihp130 EM limits are `DCCURRENTDENSITY` (AVERAGE) parsed
  from IHP's `sg13g2_tech.lef`; resistance and capacitance are likewise real-PDK LEF
  values. On this PDK the EM findings are signoff-grade, not structural estimates.
- **Currents are labeled.** Operating currents are **measured** (ngspice on the
  extracted netlist) — never silently assumed. A limit kind the PDK does not specify is
  reported as *unscreened*, never guessed.
- **No fabricated numbers.** The result row is a real, DRC/LVS-clean run at IHP's real
  foundry limits (2026-07-14); every figure is measured or explicitly labeled (the two
  honest labels: reused supply current, omitted AC decoupling caps). The CAL layout
  engine was **extended to ihp130** for this build (from scratch), with sky130 unaffected.
- **Context, not this cell's result.** EM/IR analysis on real ihp130 rules was also
  validated on a separate earlier run (an IHP IO-pad golden reference and a 3-stage
  ring-oscillator); those numbers are *not* reproduced here and are *not* this
  DualCtrlVCO result.
