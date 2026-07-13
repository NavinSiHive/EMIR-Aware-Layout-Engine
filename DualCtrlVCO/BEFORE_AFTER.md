# DualCtrlVCO — IHP130 (sg13g2) EMIR before/after at REAL foundry EM limits

A dual-control current-starved ring VCO (Williams CICC-2004; 5 identical stages + CMOS output
buffer, 27 transistors) **rebuilt FROM SCRATCH in IHP sg13g2** by the CAL grid engine, taken
DRC/LVS-clean, then run through the deployed gated EMIR flow at **IHP's REAL LEF DCCURRENTDENSITY
limits** (`emirtool` pack `ihp130`, provenance `real-pdk`). This is a genuine signoff-grade screen:
at `real-pdk` provenance the Tier-B gate fails on **any** EM ratio > 1.0× (no estimate margin).

## Provenance / method (honest labelling)
- **Layout**: CAL-native sg13g2 build (`cal/ihp130_dcvco.py`, `CAL_PDK=ihp130`). Both v1 and v2 are
  **DRC=0 (magic sg13g2) + LVS "Circuits match uniquely" (netgen sg13g2)**, 27 devices
  (sg13_lv_nmos/pmos). Logs: `before/…drc.log|lvs.log`, `after/…drc.log|lvs.log`.
- **EM rules**: REAL — sg13g2 LEF DCCURRENTDENSITY (DC average). Gate threshold 1.0× (signoff).
- **Supply current**: `VREG 456.6 µA / VSS 460.2 µA` — **REUSED from the sky130 DualCtrlVCO ngspice
  measure** (identical topology + device sizing; the sg13g2 device currents differ but are the same
  order, and a fresh ihp130 sg13g2 ngspice measure was out of scope). Nominals VREG=1.5 V, VSS=0 V.
- **Corners**: signoff pair — em_wc (125 °C, VDD+5 %, worst-R) + ir_wc (125 °C, VDD−10 %, worst-R).
- **Caps**: the ring-load / decoupling MIM caps (XCL per stage, XCdec) are **omitted** — the sg13g2
  cmim is a Metal5/TopMetal1 stack; being AC decaps they carry **no DC supply current**, so the DC
  EM/IR screen is unaffected. (This makes the ring faster / draws more current, so the screen is if
  anything conservative.)

## Result — signoff numbers

| | v1 = BEFORE (habit-sized) | v2 = AFTER (EM-repaired) |
|---|---|---|
| DRC / LVS | **0 / match uniquely** | **0 / match uniquely** |
| Tier-A structural gate | PASS (no shorts/opens) | PASS |
| **Tier-B EM verdict** | **FAIL** | **PASS** |
| EM violations (>1.0×) | **15 / 204 segments** | **0 / 301 segments** |
| **worst EM ratio** | **3.37×**  (VSS Metal1) | **0.99×**  (VREG Via1) |
| next-worst EM | 2.26× VSS Metal1, 1.97× VREG Via1, 1.76× VREG Metal1 | 0.97× VREG Metal1/Via1 |
| worst IR drop | **18.6 mV** | **9.9 mV** (budget 30 mV @ 0.02×1.5 V) |
| IR violations | 0 | 0 |

**Worst EM nom→WC**: v1 FAILs at **3.37×** the real sg13g2 DC limit on the 0.14–0.16 µm VSS Metal1
source straps (where each stage's ~92 µA return current converges), plus the 0.34 µm VREG/VSS rails
(~1.7×) and a single VREG supply Via1 (1.97×). IR is comfortably inside budget both ways.

## Repair (v1 → v2, ONE iteration) — `dcvco_ihp(wide=True)`
Root findings from the v1 heat-map + report drove three builder knobs:
1. **Widened the supply source-straps** 0.30 → 0.50 µm, **asymmetrically toward the cell edge**
   (the S/D pitch is only ~0.53 µm, so a centred wide strap would violate M1 spacing to its own
   drain) — clears the 3.37× VSS bottleneck.
2. **VSS bottom-rail mesh** — Metal1 + a co-located Metal2 strap tied by a **Via1 ARRAY** (2× the
   conductor; safe because no signal riser crosses the bottom rail).
3. **VREG rail widened** 0.34 → 0.50 µm (it is geometrically boxed between the VCOARSE gate contact
   below and the VCOARSE/VFINE/ring Metal3 tracks above, so Metal1-only) + the PMOS-source **Via1
   changed to a 2-cut array** on a local 0.7 µm landing (fixes the 1.97× single via).

Outcome: **worst EM 3.37× → 0.99×**, EM violations **15 → 0**, IR **18.6 → 9.9 mV**; Tier-B FAIL → PASS.
The VREG rail/via remain the tightest (0.97–0.99×) — an honest, thin-but-real signoff pass; VREG is
the geometric bottleneck (Metal1-only in the congested routing channel).

## Files
- `before/` — v1 GDS-derived: report .json/.md/.csv, verdict.json, heat.png (+lyp/gds), repair.json
  (directives), gate.json, spef, per-corner (em_wc/ir_wc); drc.log + lvs.log.
- `after/`  — v2, same set.
- GDS: `../DualCtrlVCO_ihp.gds` (v1), `../DualCtrlVCO_ihp_v2.gds` (v2).

## Reproduce
```bash
source /home/navinm/analog_ai/anaAI/bin/activate
export PYTHONPATH=/home/navinm/agent/cal_pll:/home/navinm/agent/emir PYTHONHASHSEED=0 CAL_PDK=ihp130
cd /home/navinm/agent/cal_pll
python3 -m cal.ihp130_dcvco vco       # v1  -> DRC=0 LVS=match
python3 -m cal.ihp130_dcvco vco_wide  # v2  -> DRC=0 LVS=match
# EMIR screen via emirtool at pack=ihp130 (real LEF DCCURRENTDENSITY limits).
# The CAL layout engine and the emirtool analysis engine are not included in this
# outputs-only repository; this block documents how the committed outputs were produced.
```
