# Ethics — GEANT4

**Project:** GEANT4  
**Category:** NUCLEAR  
**Upstream:** https://github.com/geant4/geant4  
**Pinned commit:** `f3d5293d384757b8a228a099898b2b87cfa4023c`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `a22feb6550b7746b20700dda61beca1e96b0c08139a5817aa1396f29346ea2a9`  
**Date:** October 2026

## Position

GEANT4 is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
