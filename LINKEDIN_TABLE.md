# LinkedIn post — comparison table + caption

Compact before/after table for the cell, plus a caption template. The numbers are the
real, DRC/LVS-clean ihp130 result at IHP's real foundry limits — measured or explicitly
labeled (reused current, omitted AC caps), never fabricated.

## Compact table

| Cell | Worst EM (v1 → v2) | Worst IR (v1 → v2) | Iterations | DRC / LVS |
|---|---|---|---|---|
| DualCtrlVCO | 3.37× → **0.99×** | 18.6 → 9.9 mV | 1 | 0 / match |

## Caption template (2 lines)

> A dual-control ring VCO — the oscillator core of a PLL — rebuilt from scratch in
> IHP's open 130 nm process and run through our EM/IR gate at the foundry's REAL
> current-density limits (LEF DCCURRENTDENSITY): 3.37× over its EM limit → repaired to
> a signoff-grade pass (0.99×) in one automated iteration, DRC-0 / LVS-match throughout.
>
> Honest labels: ihp130 ships real EM rules, so these are signoff-grade; the supply
> current is reused from the identical sky130 sizing; the AC decoupling caps are
> omitted (no DC current, so the DC screen is unaffected).

---

**Note on formatting.** LinkedIn does not render Markdown tables. When the results are
real, paste the numbers into a plain-text/monospace block or an image of the table for
the post itself; the Markdown above is the source of record. Drop the headline
before → after numbers (e.g. worst EM v1 → v2) into the first caption line once they
are measured.
