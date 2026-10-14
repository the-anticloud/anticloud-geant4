# Educators — GEANT4

**Project:** GEANT4  
**Category:** NUCLEAR  
**Upstream:** https://github.com/geant4/geant4  
**Pinned commit:** `f3d5293d384757b8a228a099898b2b87cfa4023c`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `a22feb6550b7746b20700dda61beca1e96b0c08139a5817aa1396f29346ea2a9`  
**Date:** October 2026

## Teaching with GEANT4

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `a22feb6550b7746b20700dda61beca1e96b0c08139a5817aa1396f29346ea2a9` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
