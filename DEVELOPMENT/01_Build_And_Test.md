# Build and Test

**Project:** `GEANT4`
**Upstream:** https://github.com/geant4/geant4
**License:** Apache 2.0

## Quick Start

```bash
git clone https://github.com/geant4/geant4
cd geant4
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local reactor anomaly detection — fully air-gapped
2. AIOSS tamper-evident operational log (NRC audit-ready)
3. AES-256 encryption for all safety system data
4. Single-binary safety monitor deployable on NRC-certified hardware
5. Zero-cloud: all inference and logging runs on isolated network
6. GPU/CPU equalizer: real-time inference on industrial GPU or redundant CPU cluster
7. Formal verification annotations on all safety-critical code paths
8. Offline dosimetry and simulation replacing cloud physics engines

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
