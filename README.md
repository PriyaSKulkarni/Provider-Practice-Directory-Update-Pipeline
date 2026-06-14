# HealthLynked AI Provider Data Pipeline

## Overview
This pipeline provides a repeatable, cost-efficient solution for updating provider directory records. Unlike monolithic update systems, our "Granular Per-Field Architecture" decouples record attributes (address, phone, specialty), allowing for independent verification and update routing.

## Core Methodology
The system utilizes a weighted trust model to determine the reliability of incoming telemetry:

**Confidence Score (C) = (W_agree × S_trust_avg) × F_risk × R_recency × M_multiplier**

- **W_agree:** Ratio of agreeing sources.
- **S_trust_avg:** Average trust weight of agreeing sources (e.g., NPI Registry vs. Practice Web).
- **F_risk:** Field-specific risk coefficient.
- **R_recency:** Time-based staleness factor.
- **M_multiplier:** Volume-based confidence boost.



## Operational Stages
1. **Prioritization:** SQL-based staleness tracking.
2. **Registry Fetch:** Dynamic NPI Registry polling.
3. **Secondary Source Fetch:** Targeted scraping for discrepancies.
4. **Normalization:** RegEx and NUCC Taxonomy mapping.
5. **Conflict Detection:** Fuzzy similarity matching.
6. **Confidence Scoring:** Independent trust-based verification.
7. **Routing:** Auto-commit for high confidence, manual queue for anomalies.
8. **Audit Trail:** Immutable logging of field-specific deltas.

## Submission Notes
- **Cost Efficiency:** Architecture defaults to "Free Tier" telemetry (Stages 1-4), reserving paid API calls only for confirmed conflicts.
- **Compliance:** Full audit trail tracking ensures HIPAA-compliant data provenance.
