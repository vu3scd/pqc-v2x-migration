# Post-Quantum Cryptography Migration in V2X and VANET Systems

## Overview
Vehicular communication security was designed for a world where elliptic curve cryptography (ECC) was considered computationally secure for the foreseeable future. That assumption is now weakening with the emergence of quantum computing. This repository presents a structured, implementation-aware approach to migrating V2X and VANET systems toward post-quantum cryptographic resilience.

## Why This Matters

* Vehicles operate for 12–20 years, far exceeding typical IT system lifecycles
* Quantum threats are expected within that operational window
* Current V2X security (IEEE 1609.2, ETSI C-ITS) relies heavily on ECC
* “Harvest now, decrypt later” is already a real-world threat model

## What This Repository Covers

* Current V2X cryptographic baseline and its limitations
* PQC algorithm applicability (ML-KEM, ML-DSA, FALCON, SPHINCS+)
* Migration priorities across infrastructure and vehicle layers
* Real-world constraints:
  * latency (10 ms V2V requirements)
  * bandwidth limitations
  * embedded hardware constraints
* Phased migration roadmap (2024–2035+)

## Key Position

Full PQC migration for high-frequency V2V safety messaging is not immediately achievable within current real-time and hardware constraints.
Hybrid cryptography (ECC + PQC) is not a temporary workaround—it is the correct architectural approach for long-lifecycle vehicular systems.

## Migration Principles

* Start with infrastructure (SCMS / CCMS, OTA, backend systems)
* Introduce PQC in key exchange before signatures
* Maintain hybrid cryptographic stacks during transition
* Design for crypto-agility from the outset
* Align with regulatory frameworks (ISO/SAE 21434, UNECE R155)

## Current Reality (2026–2030)

* PQC viable for:
  * Certificate Authorities
  * OTA signing
  * Backend TLS (V2C)
* Hybrid ECC + PQC for:
  * V2I communications
* ECC remains dominant for:
  * V2V safety beaconing

## Challenges

* Latency vs cryptographic overhead
* Certificate size inflation
* Lack of PQC-capable automotive-grade hardware
* SCMS / CCMS migration complexity
* Regulatory fragmentation
* Privacy implications under larger certificate models

## Future Work

* Lightweight PQC for V2V environments
* PQC-aware pseudonym certificate design
* Automotive-grade PQC hardware acceleration
* Cross-border trust models and interoperability
* Integration with EV charging (ISO 15118, V2G ecosystems)

## Disclaimer

This repository reflects ongoing analysis based on publicly available standards, research, and implementation constraints. The content is intended for technical discussion and will evolve as standards and real-world deployments mature.

## Author

Sumit Chouhan
Cybersecurity | V2X | EVSE | Post-Quantum Readiness
