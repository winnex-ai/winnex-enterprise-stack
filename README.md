# Winnex Enterprise Stack

> **Research Foundation for the Regulated AI Market.**
> Estamos construindo as fundacoes de um mercado que ainda nem existe.

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL%201.1-yellow)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21107295.svg)](https://doi.org/10.5281/zenodo.21107295)
[![Contact](https://img.shields.io/badge/Contact-pay@winnex.ai-blue)](mailto:pay@winnex.ai)

---

## ⚠️ Important: What This Represents

This repository is a **market-scoping document and product blueprint**, not a finished product. It reflects our vision for what regulated enterprise AI search should look like, built on 2+ years of mathematical research (11 Zenodo records, 15+ public benchmarks, validated zero-violation bounds on SIFT-1M).

**The code that exists today** -- the Madhava audit engine (C++), the smart contracts (Solidity), the integration layers (Python) -- is published in our other repositories. The enterprise compliance dashboard, SOC2 certification package, and production integrations described here are **under development and planned**, not yet delivered.

We are sharing this publicly because:
1. **We are seeking strategic partners and investors** to bring this vision to production
2. **We believe in radical transparency** -- investors should see exactly what exists and what doesn't
3. **The regulated AI market is in its infancy** -- the foundations must be built before the market arrives

### Our Thesis

> The regulated enterprise AI market (EU AI Act, LGPD, HIPAA, Basel III) is a multi-billion-dollar opportunity that does not exist today. It will emerge over the next 3-5 years as regulators begin enforcing AI transparency requirements. We have built the mathematical foundations. We are now building the commercial layer. We are looking for partners to build it with us.

---

## The Market Thesis

Enterprise AI systems in regulated environments face a fundamental requirement that no current technology satisfies: **the ability to prove, mathematically, that every search decision was correct.**

[![License: BSL 1.1](https://img.shields.io/badge/License-BSL%201.1-yellow)](LICENSE)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21107295.svg)](https://doi.org/10.5281/zenodo.21107295)
[![Zenodo](https://img.shields.io/badge/Zenodo-10.5281%2Fzenodo.21106604-1682D4?logo=zenodo)](https://doi.org/10.5281/zenodo.21106604)
[![GitHub](https://img.shields.io/badge/GitHub-Audit%20Module-181717?logo=github)](https://github.com/winnex-ai/winnex-audit-cpp)
[![Kaggle](https://img.shields.io/badge/Kaggle-Benchmark-20BEFF?logo=kaggle)](https://www.kaggle.com/code/kleniopadilha/winnex-definitive-benchmark-v1-0)
[![Contact](https://img.shields.io/badge/Contact-pay@winnex.ai-blue)](mailto:pay@winnex.ai)

---

## The Problem

Enterprise AI systems deployed in regulated environments face a fundamental requirement that no current vector search technology satisfies: **the ability to prove, mathematically, that every search decision was correct.**

| Scenario | The Question | Current Answer |
|----------|-------------|----------------|
| **Bank audit** (BACEN, Basel III) | "Why was this transaction flagged?" | "The HNSW graph returned it." — Not defensible |
| **Legal e-discovery** (e-Discovery) | "Prove you found all relevant documents." | "The model returned these 10." — No proof of completeness |
| **Medical review** (HIPAA) | "Show reproducible search results." | "Different graph each run." — Non-deterministic |
| **EU AI Act compliance** (Art. 13-15) | "Explain the logic of automated decisions." | "Black box." — No mathematical audit trail |

## The Solution

**Winnex Enterprise Stack** is a mathematically provable search infrastructure layer. It wraps existing vector databases (FAISS, Pinecone, Milvus, Weaviate, Qdrant) with a **Cauchy-Schwarz bound verifier**, generating a **per-document mathematical audit trail** for every search query.

```
┌──────────────────────────────────────────────────────────────┐
│                    REGULATORY COMPLIANCE                      │
│  EU AI Act  ·  LGPD  ·  HIPAA  ·  GDPR  ·  Basel III  ·  SEC │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│              WINNEX ENTERPRISE STACK                          │
│                                                              │
│  ┌─────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Audit Engine    │  │  Compliance      │  │  Regulatory  │ │
│  │  (C++/Python)    │  │  Dashboard       │  │  Reports     │ │
│  │  Cauchy-Schwarz  │  │  Bound monitor   │  │  EU AI Act   │ │
│  │  QR-JL proofs   │  │  Violation alert │  │  SOC2-ready  │ │
│  └────────┬────────┘  └────────┬─────────┘  └──────┬───────┘ │
│           │                    │                     │        │
│  ┌────────▼────────────────────▼─────────────────────▼───────┐│
│  │              Integration Layer                             ││
│  │  FAISS  ·  Pinecone  ·  Milvus  ·  Weaviate  ·  Qdrant   ││
│  └───────────────────────────────────────────────────────────┘│
└───────────────────────────────────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│              YOUR EXISTING VECTOR INFRASTRUCTURE              │
│  PostgreSQL/pgVector  ·  Snowflake  ·  Databricks  ·  S3     │
└───────────────────────────────────────────────────────────────┘
```

---

## The Technology: Mathematical Proof as Product

### The Cauchy-Schwarz Guarantee

For any orthogonal projection P: R^D -> R^k and any vectors v (corpus), q (query):

```
|<v,q> - <Pv,Pq>| <= ||v - P^T P v|| * ||q - P^T P q||    [Cauchy-Schwarz]

<v,q> <= <Pv,Pq> + ||v - P^T P v|| * ||q - P^T P q||      [Upper bound]
```

If the upper bound falls below the top-K threshold, the document **mathematically cannot** be in the results. This is verified at build time:

```
assert ||P @ P.T - I|| < 1e-5    // Production code assert
                                // If violated: system fails explicitly
```

### The Per-Document Audit Trail

```json
{
  "audit_record": {
    "doc_id": 42042,
    "true_cosine": 0.2317,
    "projected_cosine": 0.2281,
    "pythagorean_residual": 0.0314,
    "cauchy_schwarz_bound": 0.2595,
    "threshold_10th_result": 0.4500,
    "verdict": "EXCLUDED",
    "mathematical_certainty": true,
    "regulatory_standard": "EU_AI_ACT_ARTICLE_13",
    "timestamp": "2026-07-01T10:30:00Z"
  }
}
```

Every number can be independently recomputed by a regulator, auditor, or court.

### Empirical Validation (SIFT-1M, 50K vectors)

| Metric | Winnex [64,128] | HNSW(ef=128) | IVF(nprobe=20) |
|--------|:---------------:|:------------:|:--------------:|
| **NDCG@10** | **1.000** | 1.000 | 0.987 |
| **Recall@10** | **1.000** | 1.000 | 0.980 |
| **Bound violations** | **0 in 10M pairs** | Cannot measure | Cannot measure |
| **Deterministic?** | **Yes** | No (random graph) | Yes |
| **Audit trail?** | **Per-document proof** | None | None |
| **Build time (50K)** | **0.09s** | 0.53s | <1m |
| **Build time (1M)** | **2.57s** | ~40s | <1m |
| **CPU-only?** | **Yes** | Yes | Yes |

*Full benchmark: [10.5281/zenodo.21088504](https://doi.org/10.5281/zenodo.21088504)*

---

## Products

### Winnex Audit Engine
The core C++/Python library. Drop-in compliance layer for any vector database.

- **Build**: QR-orthogonalized JL projections + residual cache. 0.09s for 50K vectors.
- **Search**: Cascaded bound refinement (64D -> 128D -> exact). NDCG=1.000.
- **Verify**: Cauchy-Schwarz violation test. Zero violations across 10M+ pairs.
- **Report**: Full JSON audit trail per query. Ready for regulatory submission.

**Open source**: [github.com/winnex-ai/winnex-audit-cpp](https://github.com/winnex-ai/winnex-audit-cpp) (BSL 1.1)

### Winnex Compliance Dashboard (Enterprise)
Real-time compliance monitoring for regulated search operations.

- **Live bound monitor**: Track violation rates across all queries
- **Alerting**: Threshold-based notifications for compliance teams
- **Audit log**: Immutable search history with mathematical proofs
- **Regulatory reports**: One-click export for EU AI Act, LGPD, HIPAA

### Winnex Regulatory Reports (Enterprise)
Certified documentation for regulatory submission.

- **EU AI Act conformity report**: Article 13-15 compliance documentation
- **LGPD audit report**: Article 20 automated decision review
- **SOC2 readiness**: Controls mapping and evidence collection
- **Custom frameworks**: BACEN, SEC, ANVISA, FDA

---

## Use Cases

### Banking & Finance
| Use Case | Requirement | Winnex Solution |
|----------|------------|-----------------|
| Credit scoring audit | Explain why loan was denied | Mathematical proof per document |
| Anti-fraud investigation | Reproducible search trails | Deterministic, auditable |
| Regulatory reporting | BACEN/SEC compliance | One-click reports |
| Risk document retrieval | Complete search proof | Cauchy-Schwarz guarantee |

### Healthcare & Pharma
| Use Case | Requirement | Winnex Solution |
|----------|------------|-----------------|
| Clinical trial matching | No missed documents | Zero bound violations |
| Medical literature review | Reproducible research | Deterministic results |
| Patient record search | HIPAA compliance | Full audit trail |
| Drug discovery | Complete prior art search | Mathematical proof of completeness |

### Legal & e-Discovery
| Use Case | Requirement | Winnex Solution |
|----------|------------|-----------------|
| Discovery response | Prove search completeness | Per-document bound proof |
| Legal hold notification | Audit trail of searches | Immutable search log |
| Expert witness testimony | Defensible search methodology | Court-ready reports |

### Government & Defense
| Use Case | Requirement | Winnex Solution |
|----------|------------|-----------------|
| Intelligence analysis | Reproducible queries | Deterministic search |
| Procurement auditing | Transparent decision-making | Full audit trail |
| Regulatory compliance | Sovereign data control | On-premise deployment |

---

## Pricing (Aspirational — Under Validation)

The pricing below represents our **target model based on market research**, not prices we can charge today. We need certifications (SOC2, ISO 27001), reference customers, and SLA track record to validate these numbers.

| Tier | Target Annual License | Prerequisites for Validation |
|------|---------------------|------------------------------|
| **Audit Engine** (BSL 1.1) | Open source | -- |
| **Enterprise Starter** | R$ 500K/year | 3 reference customers, SOC2 certification |
| **Enterprise Growth** | R$ 1M/year | 10 customers, ISO 27001, AI Act conformity |
| **Enterprise Premium** | R$ 2M/year | 25+ customers, full certification suite |
| **Custom** | Negotiable | Enterprise agreements |

### What Target Pricing Would Include (Once Developed)
- **Mathematical guarantee**: Zero bound violations (already proven in research)
- **Compliance dashboard**: Under development
- **Certification package**: SOC2 Type II, ISO 27001 — planned, not yet obtained
- **SLA**: 99.9% uptime — standard target for enterprise
- **Training**: On-site team training and certification

---

## The Market Thesis

### The Red Ocean (Commodity Vector Search)
| Dimension | Current Market |
|-----------|---------------|
| **Primary need** | Speed (sub-ms latency) |
| **Customers** | Startups, chatbots, RAG apps |
| **Competition** | FAISS (free), Pinecone, Milvus, Weaviate |
| **Pricing** | Extreme pressure (commodity) |
| **Margins** | Thin |

### The Blue Ocean (Regulated Enterprise AI)
| Dimension | Winnex Market |
|-----------|---------------|
| **Primary need** | Mathematical proof and audit trail |
| **Customers** | Banks, healthcare, gov, legal, insurance |
| **Competition** | **None** (category creator) |
| **Pricing** | Low pressure (compliance premium) |
| **Margins** | High (R$ 500K-2M/year per customer) |

### The Asymmetric Value Proposition

| Risk | Cost of Failure | Cost of Winnex License |
|------|----------------:|----------------------:|
| EU AI Act violation (4% of global revenue) | R$ 2B+ | R$ 1M/year |
| GDPR fine (20M EUR or 4% revenue) | R$ 120M+ | R$ 500K/year |
| e-Discovery class action | R$ 200M+ | R$ 1M/year |
| HIPAA non-compliance penalty | R$ 50M+ | R$ 500K/year |

**A single compliance failure costs more than 200 years of Winnex Enterprise Premium.**

---

## The 18-Month Journey

Winnex AI was built over 18 months with zero external capital. The output:

- 11 Zenodo records documenting the full mathematical and engineering journey
- 16,000+ lines of documented C++ and Python code
- Definitive benchmark: 16 methods, 12 metrics, 3 datasets (SIFT-1M real data)
- Zero bound violations across 254M+ query-vector pairs
- C++ core with Python bindings (pybind11)
- Complete enterprise pipeline (config-driven, streaming-capable, database-integrated)

**Author**: Klenio Araujo Padilha  
**Organization**: Winnex Brasil Solucoes Empresariais LTDA - ME  
**CNPJ**: 58.364.637/0001-47 | Brazil  
**Contact**: pay@winnex.ai

---

## For Investors and Strategic Partners

Winnex AI was built over **2+ years with zero external capital**. We publish our research and code publicly for three reasons:

1. **Transparency**: Every mathematical claim can be independently verified. 11 Zenodo records, 15+ Kaggle benchmarks, zero bound violations across 254M+ query-vector pairs.
2. **IP Protection**: The core algorithm (Cauchy-Schwarz proof engine) is visible for study under BSL 1.1 but requires a commercial license for production use.
3. **Market Timing**: The regulated enterprise AI market (EU AI Act, LGPD enforcement, HIPAA AI guidance) is in its infancy. We are building the foundations now so the infrastructure is ready when the market arrives.

### What We Are Seeking

| Need | Why |
|------|-----|
| **Strategic investors** | To fund certifications (SOC2, ISO 27001), enterprise dashboard development, and go-to-market |
| **Design partners (3-5)** | Enterprise clients willing to deploy the research-grade stack and help shape the product |
| **Channel partners** | Integration with compliance platforms (OneTrust, TrustArc) and Big Four consulting firms |

**What you get:** Exclusive access to the full stack, priority roadmap influence, and equity in a company building the mathematical trust layer for regulated AI.

**Read the full thesis:** [Open Letter to Investors](https://doi.org/10.5281/zenodo.21106637) (7 pages)

---

## Documentation

| Document | Description |
|----------|-------------|
| [Open Letter to Investors](https://doi.org/10.5281/zenodo.21106604) | Market thesis, technology, partnership invitation (7 pages) |
| [Audit Benchmark Supplement](https://zenodo.org/records/21106604) | EU AI Act, LGPD, HIPAA compliance mapping (9 pages) |
| [Definitive Benchmark](https://doi.org/10.5281/zenodo.21088504) | 16 methods, SIFT-1M, 12 metrics, 68KB results |
| [Kaggle Notebook](https://www.kaggle.com/code/kleniopadilha/winnex-definitive-benchmark-v1-0) | Executable benchmark on GPU P100 |
| [Audit Module Source](https://github.com/winnex-ai/winnex-audit-cpp) | C++ library, open source (BSL 1.1) |

---

## Contact

```text
Winnex Brasil Solucoes Empresariais LTDA - ME
CNPJ: 58.364.637/0001-47
Brazil

Email: pay@winnex.ai
GitHub: https://github.com/winnex-ai
Zenodo: https://doi.org/10.5281/zenodo.21106604
```

---

---



**Business Source License 1.1 (BSL 1.1)**

- Study, testing, and non-production evaluation: **permitted**
- Commercial deployment: **requires separate license agreement**
- Change Date: 2036-01-01 (GPL v2.0+ thereafter)
- Contact for licensing: **pay@winnex.ai**

---

*Winnex AI — Trust Infrastructure for Regulated Enterprise AI.*  
*18 months of research. Zero external capital. Mathematically proven.*
