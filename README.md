# Quantum-Resistant Security in V2X and VANET
### A Structured Approach to Post-Quantum Cryptography Migration

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightblue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/vu3scd/pqc-v2x-migration/releases)
[![Article](https://img.shields.io/badge/format-Technical%20Article-green.svg)](https://github.com/vu3scd/pqc-v2x-migration/blob/main/Quantum-Resistant%20security%20in%20V2X%20and%20VANET.pdf)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19488047.svg)](https://doi.org/10.5281/zenodo.19488047)
[![PQC](https://img.shields.io/badge/topic-Post--Quantum%20Cryptography-purple.svg)](https://csrc.nist.gov/projects/post-quantum-cryptography)

> **Author:** Sumit Chouhan, TMIET 
> **Published:** April 2026  
> **Read the full article:** [PDF →](https://github.com/vu3scd/pqc-v2x-migration/blob/main/Quantum-Resistant%20security%20in%20V2X%20and%20VANET.pdf)

---

## Overview

Vehicular communication security was designed for a world where elliptic curve cryptography (ECC) was considered computationally secure for the foreseeable future. That assumption is now weakening with the emergence of quantum computing.

This repository presents a structured, implementation-aware approach to migrating V2X and VANET systems toward post-quantum cryptographic resilience covering standards, use cases, geographic boundaries, deployment architecture, technical challenges, and a phased roadmap grounded in what is actually achievable given today's hardware and protocol constraints.

---

## Why this matters

| Factor | Reality |
|--------|---------|
| Vehicle service life | 12–20 years - well into the CRQC risk window |
| Current V2X security | IEEE 1609.2 and ETSI C-ITS both rely entirely on ECDSA P-256 |
| Quantum threat timeline | CRQC capability estimated 2029–2035 by national security agencies |
| Harvest now, decrypt later | Already an active threat model for long-lived V2X credentials |
| NIST PQC status | FIPS 203/204/205 finalized August 2024 — migration clock has started |

---

## What this repository covers

### 1. Current cryptographic baseline
The full V2X security stack: IEEE 1609.2 (WAVE/DSRC), ETSI TS 103 097 (C-ITS), SAE J2945/1, 3GPP C-V2X, ISO/SAE 21434, and UNECE WP.29 and exactly where each standard is quantum-vulnerable.

### 2. PQC algorithm applicability
How NIST's finalized algorithms map onto V2X requirements:

| Algorithm | FIPS | Replaces | V2X Role | Fit |
|-----------|------|----------|----------|-----|
| ML-KEM (Kyber) | 203 | ECDH / ECIES | Session key establishment | ✅ Good |
| ML-DSA (Dilithium) | 204 | ECDSA | CA signing, OTA, V2I | ⚠️ Medium — large sigs |
| SLH-DSA (SPHINCS+) | 205 | ECDSA (CA only) | Root/Sub-CA only | ⚠️ CA layer only |
| FN-DSA (FALCON) | 206* | ECDSA | V2V beaconing candidate | ✅ Best V2V fit |

### 3. Use Case priority classification
Nine V2X use cases ranked by data sensitivity, message frequency, and consequence of compromise from PKI credential provisioning (Critical) to in-vehicle infotainment (Lower).

### 4. Geographic and Regional boundaries
How PQC deployment scope should be defined by region - North America (NIST/SCMS), Europe (ETSI/CCMS), China/APAC (OSCCA/GB/T divergence), GCC/Middle East (greenfield opportunity), and the cross-border interoperability gap that no framework currently addresses.

### 5. Layer-by-Layer migration map
Where PQC must be applied across the V2X stack - PKI/trust, message authentication, key exchange, bulk encryption, OTA updates, TLS backend, and HSM/OBU hardware - with migration difficulty rated for each.

### 6. Technical challenges
Seven structural challenges that must be confronted:
- **C1** : Latency vs. security overhead (10 ms V2V verification target)
- **C2** : Certificate and message size inflation
- **C3** : Hardware lifecycle and OBU constraints (ECC HSMs cannot be patched)
- **C4** : SCMS/CCMS infrastructure migration complexity
- **C5** : Regulatory and standards fragmentation (NIST, ETSI, 3GPP, SAE, OSCCA)
- **C6** : Privacy preservation under larger PQC certificate models
- **C7** : Algorithm maturity and side-channel risk on automotive hardware

### 7. Phased migration roadmap (2024–2038)

```
Phase 1 (Now–2026)   : Cryptographic inventory, SCMS planning, mandate PQC-capable HSMs in new OBU RFPs
Phase 2 (2026–2028)  : PQC root/sub-CA deployment, OTA signing migrated to ML-DSA/SLH-DSA
Phase 3 (2028–2031)  : Pseudonym certs: hybrid ECC + ML-DSA, V2I SPaT/MAP PQC at RSUs
Phase 4 (2031–2035)  : V2V beaconing (BSM/CAM) PQC rollout as fleet threshold crossed
Phase 5 (Post-2035)  : ECC deprecated per NIST IR 8547, full PQC-native fleet
```

## Key positions

**Hybrid cryptography is the correct architectural approach not a temporary workaround.**  
Running ML-KEM alongside ECDH and ML-DSA alongside ECDSA during the transition provides security against both classical and quantum adversaries simultaneously. For long-lifecycle systems like vehicles, this is the only architecturally sound path.

**Full PQC migration of V2V safety beaconing will not be complete before 2035.**  
The 10 Hz broadcast rate, 10 ms verification requirement, and constrained OBU hardware are hard physical constraints. Current PQC implementations do not meet them without dedicated hardware acceleration that does not yet exist in automotive-grade silicon at scale.

**The governance gap is larger than the technical gap.**  
No single entity owns the SCMS or CCMS PQC root CA transition. Until a governance authority is defined for coordinating that migration, technical readiness is not enough.

**Infrastructure first, always.**  
SCMS/CCMS, OTA signing pipelines, and V2C backend TLS are achievable now and represent the highest-value near-term targets.

## Current Reality (2026–2030)

| Layer | Status |
|-------|--------|
| Root/Sub-CA (SCMS/CCMS) | ✅ PQC migration viable now (ML-DSA / SLH-DSA) |
| OTA firmware signing | ✅ PQC migration viable now (ML-DSA) |
| V2C backend TLS | ✅ Hybrid ML-KEM + ECDH deployable now |
| V2I messages (SPaT/MAP) | ⚠️ Hybrid at upgraded RSUs - 2028+ |
| Pseudonym certificates | ⚠️ Hybrid ECC + ML-DSA - 2028+ (format redesign needed) |
| V2V safety beaconing (BSM/CAM) | 🔴 ECC dominant through 2030+, hardware constraints unresolved |
| OBU hardware (HSM) | 🔴 New PQC-capable silicon required - mandate from 2026 in new RFPs |

## The Hard Open Questions

1. **Is the V2V beaconing problem actually solvable within real-time constraints?**  
   Current research cannot consistently meet 10 ms verification at 10 Hz on automotive-grade embedded hardware without dedicated PQC accelerators. This is the central unresolved technical question.

2. **Who coordinates the SCMS root CA transition?**  
   The SCMS/CCMS migration is a governance event, not just a technical one. No single authority currently owns it.

3. **Does geographic scoping create interoperability dead zones?**  
   Divergent regional timelines will create certificate validation failures at borders without hybrid certificates and cross-domain trust agreements neither of which are standardised yet.

4. **Are NIST's general-purpose PQC algorithms right for V2X specifically?**  
   ML-KEM and ML-DSA were designed as general-purpose algorithms. V2X is latency-critical, bandwidth-constrained, and hardware-limited. V2X-specific lightweight PQC variants remain in academic research, not standardised.

## Future Work Areas

- Lightweight PQC variants specifically for V2V constrained environments
- PQC-aware pseudonym certificate format design (IEEE 1609.2 / ETSI TS 103 097 extensions)
- Automotive-grade PQC hardware acceleration benchmarking
- Cross-border trust model design and interoperability frameworks
- Integration with EV charging infrastructure (ISO 15118 / V2G ecosystems- a parallel and largely unaddressed PQC problem)
- Governance framework proposals for SCMS/CCMS PQC root CA transition

## References

Key standards and research referenced in this work:

- [NIST PQC Standardization Project](https://csrc.nist.gov/projects/post-quantum-cryptography) - ML-KEM, ML-DSA, SLH-DSA, FN-DSA
- [NIST FIPS 203](https://doi.org/10.6028/NIST.FIPS.203) - ML-KEM specification
- [NIST FIPS 204](https://doi.org/10.6028/NIST.FIPS.204) - ML-DSA specification
- [NIST FIPS 205](https://doi.org/10.6028/NIST.FIPS.205) - SLH-DSA specification
- [NIST IR 8547](https://csrc.nist.gov/pubs/ir/8547/ipd) - Transition timeline and deprecation schedule
- [IEEE 1609.2-2022](https://standards.ieee.org/ieee/1609.2/10323/) - WAVE security services standard
- [ETSI TS 103 097](https://www.etsi.org/deliver/etsi_ts/103000_103099/103097/) - C-ITS security header and certificate formats
- [ISO/SAE 21434:2021](https://www.iso.org/standard/70918.html) - Road vehicles cybersecurity engineering
- [AI-Driven PQC for V2X (arXiv 2025)](https://arxiv.org/html/2510.08496v1)
- [Lightweight Key Agreement for V2X — Kyber+Saber (MDPI 2025)](https://www.mdpi.com/1424-8220/25/22/6938)
- [Adaptive PQC for 6G Vehicular Networks (arXiv 2026)](https://arxiv.org/html/2602.01342v1)

Full reference list with descriptions is included in the PDF article.

Content will be updated as standards and real-world deployments mature.
