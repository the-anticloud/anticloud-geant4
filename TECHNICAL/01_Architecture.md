# Technical Architecture — GEANT4

**Upstream:** [https://github.com/geant4/geant4](https://github.com/geant4/geant4)
**License:** Apache 2.0
**Category:** NUCLEAR
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Particle/nuclear physics simulation toolkit

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local reactor anomaly detection — fully air-gapped
2. AIOSS tamper-evident operational log (NRC audit-ready)
3. AES-256 encryption for all safety system data
4. Single-binary safety monitor deployable on NRC-certified hardware
5. Zero-cloud: all inference and logging runs on isolated network
6. GPU/CPU equalizer: real-time inference on industrial GPU or redundant CPU cluster
7. Formal verification annotations on all safety-critical code paths
8. Offline dosimetry and simulation replacing cloud physics engines

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_geant4.spec` or `go build -o geant4`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |