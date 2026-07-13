# EMIR report — DualCtrlVCO_ihp_v2 (ihp130)

EM rules provenance: **real-pdk — LEF DCCURRENTDENSITY AVERAGE (DC only; rms/peak unspecified)**

- EM: 0 violation(s) / 301 segments; worst 0.99x
- IR: 0 net(s) over budget; worst drop 9.9 mV
- verdict: CLEAN

| segment | layer | w/cuts | I_avg | Imax_avg | worst | tier |
|---|---|---|---|---|---|---|
| VREG:190 | Via1 | 1cut | 2.147e-04 | 2.159e-04 | 0.99x | marginal |
| VREG:80 | Metal1 | 0.5um | 2.623e-04 | 2.699e-04 | 0.97x | marginal |
| VREG:192 | Via1 | 1cut | 2.089e-04 | 2.159e-04 | 0.97x | marginal |
| VREG:79 | Metal1 | 0.5um | 2.576e-04 | 2.699e-04 | 0.95x | marginal |
| VREG:78 | Metal1 | 0.5um | 2.401e-04 | 2.699e-04 | 0.89x | marginal |
| VREG:77 | Metal1 | 0.5um | 2.354e-04 | 2.699e-04 | 0.87x | marginal |
| VREG:172 | Metal2 | 0.5um | 4.566e-04 | 5.398e-04 | 0.85x | marginal |
| VREG:171 | Metal2 | 0.5um | 4.566e-04 | 5.398e-04 | 0.85x | marginal |
| VREG:76 | Metal1 | 0.5um | 2.118e-04 | 2.699e-04 | 0.78x | watch |
| VREG:75 | Metal1 | 0.5um | 2.071e-04 | 2.699e-04 | 0.77x | watch |
| VREG:74 | Metal1 | 0.5um | 1.757e-04 | 2.699e-04 | 0.65x | watch |
| VREG:73 | Metal1 | 0.5um | 1.710e-04 | 2.699e-04 | 0.63x | watch |

IR VREG: worst drop 9.86 mV at `VREG:8`; delivered 0.46 mA

IR VSS: worst drop 0.54 mV at `VSS:24`; delivered -0.46 mA
