# EMIR report — DualCtrlVCO_ihp (ihp130)

EM rules provenance: **real-pdk — LEF DCCURRENTDENSITY AVERAGE (DC only; rms/peak unspecified)**

- EM: 15 violation(s) / 204 segments; worst 3.37x
- IR: 0 net(s) over budget; worst drop 18.6 mV
- verdict: VIOLATIONS

| segment | layer | w/cuts | I_avg | Imax_avg | worst | tier |
|---|---|---|---|---|---|---|
| VSS:32 | Metal1 | 0.1579um | 2.871e-04 | 8.523e-05 | 3.37x | high |
| VSS:36 | Metal1 | 0.1421um | 1.731e-04 | 7.670e-05 | 2.26x | over |
| VREG:165 | Via1 | 1cut | 4.243e-04 | 2.159e-04 | 1.97x | over |
| VREG:84 | Metal1 | 0.34um | 3.228e-04 | 1.835e-04 | 1.76x | over |
| VREG:83 | Metal1 | 0.34um | 3.182e-04 | 1.835e-04 | 1.73x | over |
| VREG:82 | Metal1 | 0.34um | 3.136e-04 | 1.835e-04 | 1.71x | over |
| VREG:81 | Metal1 | 0.34um | 3.090e-04 | 1.835e-04 | 1.68x | over |
| VSS:26 | Metal1 | 0.34um | 2.893e-04 | 1.835e-04 | 1.58x | over |
| VREG:80 | Metal1 | 0.34um | 2.860e-04 | 1.835e-04 | 1.56x | over |
| VREG:79 | Metal1 | 0.34um | 2.813e-04 | 1.835e-04 | 1.53x | over |
| VSS:25 | Metal1 | 0.34um | 2.761e-04 | 1.835e-04 | 1.50x | over |
| VREG:78 | Metal1 | 0.34um | 2.352e-04 | 1.835e-04 | 1.28x | over |
| VREG:77 | Metal1 | 0.34um | 2.306e-04 | 1.835e-04 | 1.26x | over |
| VSS:22 | Metal1 | 0.34um | 2.104e-04 | 1.835e-04 | 1.15x | over |
| VSS:21 | Metal1 | 0.34um | 1.972e-04 | 1.835e-04 | 1.07x | over |
| VREG:76 | Metal1 | 0.34um | 1.660e-04 | 1.835e-04 | 0.90x | marginal |
| VREG:75 | Metal1 | 0.34um | 1.614e-04 | 1.835e-04 | 0.88x | marginal |
| VSS:18 | Metal1 | 0.34um | 1.578e-04 | 1.835e-04 | 0.86x | marginal |
| VREG:74 | Metal1 | 0.34um | 1.568e-04 | 1.835e-04 | 0.85x | marginal |
| VREG:73 | Metal1 | 0.34um | 1.522e-04 | 1.835e-04 | 0.83x | marginal |
| VSS:17 | Metal1 | 0.34um | 1.446e-04 | 1.835e-04 | 0.79x | watch |
| VREG:72 | Metal1 | 0.34um | 1.291e-04 | 1.835e-04 | 0.70x | watch |
| VREG:71 | Metal1 | 0.34um | 1.245e-04 | 1.835e-04 | 0.68x | watch |

IR VREG: worst drop 18.64 mV at `VREG:8`; delivered 0.46 mA

IR VSS: worst drop 3.50 mV at `VSS:14`; delivered -0.46 mA
