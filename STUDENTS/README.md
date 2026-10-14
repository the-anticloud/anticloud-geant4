# Students — GEANT4

**Project:** GEANT4  
**Category:** NUCLEAR  
**Upstream:** https://github.com/geant4/geant4  
**Pinned commit:** `f3d5293d384757b8a228a099898b2b87cfa4023c`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `a22feb6550b7746b20700dda61beca1e96b0c08139a5817aa1396f29346ea2a9`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `f3d5293d384757b8a228a099898b2b87cfa4023c`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `a22feb6550b7746b20700dda61beca1e96b0c08139a5817aa1396f29346ea2a9`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
