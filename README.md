<div align="center">

# 🏛️ Bhu-Sanvad — The Land Intelligence Layer

### Intelligent Land Record Digitization, Conflict Verification & Cadastral Graph Engine

**Every parcel's complete story. Every conflict backed by evidence. Every decision made by an official.**

[![SIH 2026](https://img.shields.io/badge/Smart%20India%20Hackathon-2026-FF6F00?style=for-the-badge)](https://sih.gov.in)
[![Problem Statement](https://img.shields.io/badge/PS-SIH26018-1E88E5?style=for-the-badge)](#-problem-statement)
[![Theme](https://img.shields.io/badge/Theme-Smart%20Automation-43A047?style=for-the-badge)](#-problem-statement)

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Neo4j](https://img.shields.io/badge/Neo4j-5.x-4581C3?style=flat-square&logo=neo4j&logoColor=white)](https://neo4j.com/)
[![PostGIS](https://img.shields.io/badge/PostGIS-3.4-336791?style=flat-square&logo=postgresql&logoColor=white)](https://postgis.net/)
[![Celery](https://img.shields.io/badge/Celery-Redis-37814A?style=flat-square&logo=celery&logoColor=white)](https://docs.celeryq.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![Architecture](https://img.shields.io/badge/Architecture-HITL%20Middleware-8E24AA?style=flat-square)](#-system-architecture--data-flow)

**Team ID `183252` · TEAM MATRIX123**
Sponsoring Agency: Department of Land Resources (DoLR), Ministry of Rural Development, Govt. of India

</div>

---

> [!IMPORTANT]
> **Bhu-Sanvad is a decision-support system. It never decides who legally owns land.** It reads the records, reconstructs ownership lineage, flags inconsistencies and shows the evidence. An authorised Patwari or Tehsildar makes every final decision, and each decision is written to a tamper-evident audit ledger.

---

## 📑 Table of Contents

1. [Problem Statement](#-problem-statement)
2. [Core Value Proposition](#-core-value-proposition)
3. [System Architecture & Data Flow](#-system-architecture--data-flow)
4. [Core Subsystem Breakdown](#-core-subsystem-breakdown)
   - [4.1 Vision & Indic Document Understanding](#41-vision--indic-document-understanding-pipeline)
   - [4.2 Structured Entity Extraction](#42-structured-entity-extraction--normalisation)
   - [4.3 Conflict Verification & Invariant Checking](#43-algorithmic-conflict-verification--invariant-checking)
   - [4.4 Temporal Land-Story Graph](#44-graph-based-temporal-land-story-reconstruction)
   - [4.5 Evidence-Backed RAG](#45-evidence-backed-rag-layer)
   - [4.6 Cryptographic Trust & Audit Trails](#46-cryptographic-trust--audit-trails)
   - [4.7 Human-in-the-Loop Workflow & RBAC](#47-human-in-the-loop-workflow--rbac)
5. [Repository Structure](#-repository-structure)
6. [Local Setup & Installation](#-local-setup--installation)
7. [API Reference](#-api-reference-core-endpoints)
8. [Benchmarks, Impact & National Alignment](#-benchmarks-impact--national-alignment)
9. [Roadmap](#-roadmap)
10. [Contribution Guidelines](#-contribution-guidelines)
11. [Team & License](#-team--license)

---

## 🎯 Problem Statement

India has digitised most of its land records, but those digital records still don't talk to each other. According to DoLR figures for **31 January 2026**:

| Indicator | Status |
|---|---|
| Records of Rights (RoR) computerised | **99.81 %** |
| Cadastral maps digitised | **97.42 %** |
| Villages where records are actually integrated | **84.32 %** |
| Land parcels with Bhu-Aadhaar / ULPIN | **37 crore+** |

The *current* RoR is digital. The **history behind it** is not. Old mutation registers, handwritten Jamabandi / Khatauni folios, sale deeds and inheritance orders sit in record rooms as degraded scans. Generic OCR fails on them, so nobody can quickly answer basic questions:

- *How did this parcel reach its current owner?*
- *Does the recorded area match the map?*
- *Is there a gap or contradiction in the chain of title?*

When these questions go unanswered, the gaps turn into civil litigation years later.

---

## 💡 Core Value Proposition

Bhu-Sanvad is **intelligent middleware**. It sits on top of existing infrastructure (DILRMP, Bhu-Naksha GIS, state RoR databases) and replaces none of it.

| # | Capability | What it does |
|---|---|---|
| 1 | **Adaptive Indic Vision / HTR** | Reads degraded, cursive, stamped historical revenue folios in Indian scripts |
| 2 | **Temporal Ownership Knowledge Graph** | Rebuilds each parcel's full chain of mutations, inheritances and registrations |
| 3 | **Multi-Invariant Conflict Checking** | Checks area conservation, share completeness, temporal order, lineage acyclicity and spatial consistency |
| 4 | **Evidence-Backed RAG** | Explains every flag with citations down to the page and bounding box of the original scan |
| 5 | **Cryptographic Integrity** | SHA-256 document fingerprints plus a hash-chained, append-only audit ledger |
| 6 | **Strict HITL Decision Support** | Patwari / Tehsildar dashboards; nothing is changed without an authorised human |

---

## 🏗️ System Architecture & Data Flow

```mermaid
flowchart LR
    A["📄 Raw Scans / PDFs<br/>Jamabandi · Khasra · Khatauni · Deeds"] --> B["🧹 Ingestion & Preprocessing<br/>deskew · denoise · Sauvola · stamp mask"]
    B --> C["🔤 Indic HTR + Layout Parsing<br/>IndicTrOCR / CRNN-CTC · LayoutLMv3"]
    C --> D["🧩 Structured Entity Extraction<br/>schema · unit normalisation · name resolution"]
    D --> E["🕸️ Graph Builder<br/>Neo4j lineage + PostGIS parcels"]
    E --> F["⚖️ Invariant Conflict Checker<br/>I1–I7 · risk score"]
    D --> G["🔎 FAISS Vector Index<br/>chunk ↔ page ↔ bbox"]
    F --> H["🧠 RAG Reasoner<br/>citation-locked LLM"]
    G --> H
    H --> I["👩‍💼 HITL Dashboard<br/>Patwari · Tehsildar"]
    I --> J["🔄 DILRMP / State RoR Sync<br/>approved corrections only"]
    B -.-> L[("🔐 SHA-256 Hash-Chained Ledger")]
    F -.-> L
    I -.-> L
```

**Async execution model.** Ingestion returns immediately with a `job_id`. The heavy stages run as a Celery chain on Redis-backed queues, split across three worker pools (`vision`, `graph`, `rag`), so GPU work and I/O work scale independently.

```text
POST /ingest ──► FastAPI ──► Celery chain:
                               preprocess.s()  ─► htr_layout.s() ─► extract.s()
                                                                      │
                                           ┌──────────────────────────┴───────────┐
                                           ▼                                      ▼
                                   graph.upsert.s() ─► conflicts.check.s()   rag.index.s()
                                                              │
                                                              ▼
                                                    ledger.append.s()  ─► dashboard queue
```

```mermaid
sequenceDiagram
    autonumber
    participant P as Patwari
    participant API as FastAPI
    participant CC as Conflict Checker
    participant RAG as RAG Reasoner
    participant T as Tehsildar
    participant L as Audit Ledger
    P->>API: Upload folio scan
    API->>L: append(INGEST, sha256)
    API->>CC: run invariants on affected parcels
    CC-->>API: conflict C-0192 (I5 chain break, risk 78)
    API->>RAG: explain(C-0192)
    RAG-->>API: explanation + citations (doc, page, bbox)
    API-->>T: review queue item with evidence overlay
    T->>API: decision = CONFIRM_DISCREPANCY + remarks
    API->>L: append(DECISION, officer_id, signature)
```

---

## 🔬 Core Subsystem Breakdown

### 4.1 Vision & Indic Document Understanding Pipeline

Historical revenue folios combine faded ink, bleed-through, ruled tables, overlapping rubber stamps and cursive Devanagari / Gurmukhi / Bengali / Kannada handwriting. The pipeline has five stages:

| Stage | Technique | Why |
|---|---|---|
| **Rasterise** | PyMuPDF at 300 DPI, 8-bit grayscale + original RGB retained | HTR accuracy drops sharply below ~250 DPI |
| **Deskew** | Hough line detection on ruled table lines → rotation by median angle | Register folios are rarely scanned straight |
| **Denoise** | Non-local means (`cv2.fastNlMeansDenoising`, h=10) | Removes paper grain without eroding thin strokes |
| **Stamp / seal masking** | HSV colour segmentation for red/violet/blue ink + morphological open/close; Hough-circle fallback on grayscale | Seals overlapping text are the #1 cause of OCR garbage |
| **Binarise** | **Sauvola adaptive thresholding** | Handles uneven illumination and fading within one page |

**Sauvola threshold** for each pixel, computed over a local window $w \times w$:

```math
T(x,y) = m(x,y)\left[1 + k\left(\frac{s(x,y)}{R} - 1\right)\right]
```

where $m$ and $s$ are the local mean and standard deviation, $R = 128$ is the dynamic range of $s$, and $k \in [0.2, 0.5]$ is tuned per scan batch. We use $w = 25$ and $k = 0.34$ by default.

```python
# models/pipeline/preprocess.py
import cv2
import numpy as np
from skimage.filters import threshold_sauvola

STAMP_HSV_RANGES = [
    ((0, 70, 50), (10, 255, 255)),      # red (low hue)
    ((160, 70, 50), (180, 255, 255)),   # red (high hue)
    ((110, 60, 40), (150, 255, 255)),   # blue / violet
]

def mask_stamps(bgr: np.ndarray) -> np.ndarray:
    """Return a binary mask of coloured seal/stamp regions."""
    hsv = cv2.cvtColor(bgr, cv2.COLOR_BGR2HSV)
    mask = np.zeros(hsv.shape[:2], dtype=np.uint8)
    for lo, hi in STAMP_HSV_RANGES:
        mask |= cv2.inRange(hsv, np.array(lo), np.array(hi))
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel, iterations=1)
    mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel, iterations=2)
    return mask

def preprocess(bgr: np.ndarray, window: int = 25, k: float = 0.34) -> np.ndarray:
    stamps = mask_stamps(bgr)
    gray = cv2.cvtColor(bgr, cv2.COLOR_BGR2GRAY)
    gray = cv2.inpaint(gray, stamps, 3, cv2.INPAINT_TELEA)       # fill seal pixels
    gray = cv2.fastNlMeansDenoising(gray, h=10)
    thresh = threshold_sauvola(gray, window_size=window, k=k)
    return ((gray > thresh) * 255).astype(np.uint8)
```

**Layout parsing.** A LayoutLMv3 model fine-tuned for token/region classification detects the logical fields of each register format: `khasra_no`, `khata_no`, `owner_block`, `area_cell`, `mutation_ref` and `remarks`. Each field becomes a bounding box `[x0, y0, x1, y1]` in page pixel space. Text is then recognised **per field crop**, which keeps tabular structure intact.

**Handwritten text recognition (HTR).** Two recognisers run on each crop:

- **IndicTrOCR**, a transformer encoder–decoder fine-tuned on Indic handwriting, is the primary recogniser.
- **CRNN + CTC** is a lightweight fallback and a second opinion.

The CRNN is trained with the Connectionist Temporal Classification objective:

```math
\mathcal{L}_{CTC} = -\ln \sum_{\pi \in \mathcal{B}^{-1}(y)} \prod_{t=1}^{T} p(\pi_t \mid x)
```

**Two-model agreement gate.** The two outputs are compared with Character Error Rate (CER). A field is **auto-accepted** only when both of these hold:

```math
\text{CER}(\hat y_{\text{TrOCR}}, \hat y_{\text{CRNN}}) \le 0.05 \quad \land \quad \bar p_{\text{field}} \ge \tau_{\text{accept}} = 0.90
```

Otherwise the field goes to the HITL correction queue with both readings side by side. Digit-heavy fields (khasra number, area) use a stricter gate: CER = 0.

---

### 4.2 Structured Entity Extraction & Normalisation

Each recognised page becomes a canonical `LandRecord` document. It is stored in MongoDB, and its derived nodes and edges are written to Neo4j.

```json
{
  "record_id": "rec_01J9Z4K7X2B8",
  "ulpin": "MH27A0B4C9D1E2",
  "state_code": "27",
  "district": "Pune",
  "tehsil": "Haveli",
  "village_code": "272104",
  "record_type": "MUTATION_ENTRY",
  "khata_no": "1187",
  "khasra_no": "412/2",
  "owners": [
    { "person_id": "per_5521", "name_native": "सुरेश बाबूराव पाटील", "name_translit": "suresh baburao patil", "relation": "S/O Baburao", "share": 0.5 },
    { "person_id": "per_5522", "name_native": "रमेश बाबूराव पाटील", "name_translit": "ramesh baburao patil", "relation": "S/O Baburao", "share": 0.5 }
  ],
  "area": { "value": 1.2, "unit": "HECTARE", "area_sqm": 12000.0 },
  "land_class": "AGRICULTURAL_IRRIGATED",
  "mutation": { "mutation_no": "M-3391", "date": "1998-06-14", "type": "INHERITANCE", "from_person_ids": ["per_5510"] },
  "source": { "doc_id": "doc_7f3a91c2", "page": 3, "bbox": [412, 1180, 1630, 1342], "sha256": "9b1c0e4f5d…a27e" },
  "confidence": { "khasra_no": 0.99, "owners": 0.93, "area": 0.97 },
  "review_status": "AUTO_ACCEPTED"
}
```

> [!NOTE]
> All names, IDs and ULPINs in this README are **synthetic**. The prototype never uses real citizen data.

**Area unit normalisation.** Local units differ by state, and even the same unit (e.g. *bigha*) has different sizes in different states. Conversion factors live in per-state config files (`config/units/<state_code>.yaml`) and are never hard-coded.

| Unit | m² | Scope |
|---|---|---|
| Hectare | 10,000 | National |
| Acre | 4,046.86 | National |
| Guntha | 101.17 | Maharashtra, Karnataka (40 guntha = 1 acre) |
| Bigha / Biswa / Kanal / Marla | *state-specific* | Loaded from `config/units/<state_code>.yaml` |

**Owner-name resolution.** The same person appears as "Suresh B. Patil", "सुरेश बाबूराव पाटील" and "Sureshrao Patil" across decades of records. Resolution works in three steps:

1. **Transliterate** native script to a common Latin key (BHASHINI transliteration API, with `indic-transliteration` as an offline fallback).
2. **Score** candidate pairs:

```math
S(a,b) = 0.6\cdot \text{JaroWinkler}(a,b) + 0.25\cdot \mathbb{1}[\text{phon}(a)=\text{phon}(b)] + 0.15\cdot \mathbb{1}[\text{relation}(a)=\text{relation}(b)]
```

3. **Decide**: $S \ge 0.92$ → auto-link; $0.75 \le S < 0.92$ → suggested link for HITL confirmation; $S < 0.75$ → distinct persons.

---

### 4.3 Algorithmic Conflict Verification & Invariant Checking

Each rule is a **formal invariant** evaluated over the land-story graph. A failed invariant becomes a `Conflict` object carrying a severity, the confidence of its inputs and the evidence nodes behind it.

| ID | Invariant | Formulation | Severity $w_k$ |
|---|---|---|---|
| **I1** | Area conservation (sub-division) | $\left\lvert \sum_{i=1}^{n} A_{\text{sub},i} - A_{\text{parent}} \right\rvert \le \varepsilon$ | 30 |
| **I2** | Share completeness | $\left\lvert \sum_{j} \text{share}_j(t) - 1 \right\rvert \le 0.001 \;\; \forall t$ | 25 |
| **I3** | Temporal ordering | $T_{\text{mutation}} > T_{\text{parent\_acquisition}}$ | 20 |
| **I4** | Lineage acyclicity | $G_{\text{transfer}}$ is a DAG (no ownership cycles) | 35 |
| **I5** | Chain-of-title continuity | transferor $\in \text{Owners}(p, T_m^{-})$ | 40 |
| **I6** | Duplicate parcel | $\exists\, r_1 \ne r_2 : \text{key}(r_1)=\text{key}(r_2) \land \text{owners}(r_1) \ne \text{owners}(r_2)$ at the same $t$ | 30 |
| **I7** | Spatial consistency | $\dfrac{\lvert A_{\text{record}} - A_{\text{polygon}} \rvert}{A_{\text{polygon}}} \le \varepsilon_s$ | 20 |

The tolerance is both relative and absolute, because old records round areas:

```math
\varepsilon = \max(\varepsilon_{abs},\; \varepsilon_{rel}\cdot A_{\text{parent}}), \qquad \varepsilon_{abs}=5\,\text{m}^2,\;\; \varepsilon_{rel}=0.01,\;\; \varepsilon_s = 0.05
```

**Dispute-risk score.** Each parcel gets a transparent, bounded score. There is no black-box classifier.

```math
R_{\text{parcel}} = \min\left(100,\; \sum_{k \in \text{failed}} w_k \cdot c_k \right)
```

Here $c_k \in [0,1]$ is the minimum extraction confidence among the fields that invariant *k* used. Bands: 🟢 `0` · 🟡 `1–29` · 🔴 `30–100`.

```python
# graph/invariants.py
from dataclasses import dataclass
import networkx as nx

@dataclass
class Conflict:
    rule: str
    parcel_ulpin: str
    severity: int
    confidence: float
    evidence_record_ids: list[str]
    detail: dict

def check_area_conservation(parent, children, eps_abs=5.0, eps_rel=0.01) -> Conflict | None:
    total = sum(c.area_sqm for c in children)
    eps = max(eps_abs, eps_rel * parent.area_sqm)
    delta = total - parent.area_sqm
    if abs(delta) <= eps:
        return None
    return Conflict(
        rule="I1_AREA_CONSERVATION",
        parcel_ulpin=parent.ulpin,
        severity=30,
        confidence=min([parent.conf_area] + [c.conf_area for c in children]),
        evidence_record_ids=[parent.record_id] + [c.record_id for c in children],
        detail={"parent_sqm": parent.area_sqm, "children_sqm": total, "delta_sqm": round(delta, 2), "tolerance_sqm": eps},
    )

def check_temporal_order(mutation, transferor_acquired_on) -> Conflict | None:
    if transferor_acquired_on is None or mutation.date > transferor_acquired_on:
        return None
    return Conflict("I3_TEMPORAL_ORDER", mutation.ulpin, 20, mutation.conf_date,
                    [mutation.record_id],
                    {"mutation_date": str(mutation.date), "transferor_acquired_on": str(transferor_acquired_on)})

def check_acyclic(transfer_edges: list[tuple[str, str]], ulpin: str) -> Conflict | None:
    g = nx.DiGraph(transfer_edges)
    try:
        cycle = nx.find_cycle(g)
    except nx.NetworkXNoCycle:
        return None
    return Conflict("I4_LINEAGE_CYCLE", ulpin, 35, 1.0, [], {"cycle": cycle})
```

**I5: chain-of-title continuity (Cypher).** This finds mutations where the transferor did not own the parcel at the time of transfer:

```cypher
MATCH (m:Mutation)-[:ON_PARCEL]->(p:Parcel), (m)-[:FROM]->(seller:Person)
WHERE NOT EXISTS {
  MATCH (seller)-[o:OWNED]->(p)
  WHERE o.from_date < m.date AND (o.to_date IS NULL OR o.to_date >= m.date)
}
RETURN p.ulpin AS ulpin, m.mutation_no AS mutation_no, seller.person_id AS seller, m.date AS date
```

**I7: spatial consistency (PostGIS).** This compares the recorded area against the Bhu-Naksha polygon:

```sql
SELECT r.ulpin,
       r.area_sqm                                   AS recorded_sqm,
       ST_Area(p.geom::geography)                   AS polygon_sqm,
       ABS(r.area_sqm - ST_Area(p.geom::geography))
         / NULLIF(ST_Area(p.geom::geography), 0)    AS rel_diff
FROM   land_records r
JOIN   cadastral_parcels p ON p.ulpin = r.ulpin
WHERE  r.is_current
AND    ABS(r.area_sqm - ST_Area(p.geom::geography))
         / NULLIF(ST_Area(p.geom::geography), 0) > 0.05;
```

---

### 4.4 Graph-Based Temporal Land-Story Reconstruction

Every parcel's history is a **time-stamped directed acyclic graph**. Persons, parcels and legal events are nodes. Ownership is an edge with a validity interval, so the graph can answer *"who owned what, when"* for any date.

```mermaid
graph LR
    P1(("Person<br/>Baburao Patil")) -- "OWNED<br/>1971 → 1998" --> K1["Parcel<br/>ULPIN MH27…E2<br/>Khasra 412"]
    M1{{"Mutation M-3391<br/>INHERITANCE · 1998-06-14"}} -- FROM --> P1
    M1 -- ON_PARCEL --> K1
    M1 -- TO --> P2(("Person<br/>Suresh Patil"))
    M1 -- TO --> P3(("Person<br/>Ramesh Patil"))
    P2 -- "OWNED 0.5<br/>1998 → 2014" --> K1
    P3 -- "OWNED 0.5<br/>1998 → present" --> K1
    K1 -- "SUBDIVIDED_INTO<br/>2014" --> K2["Parcel<br/>Khasra 412/1"]
    K1 -- "SUBDIVIDED_INTO<br/>2014" --> K3["Parcel<br/>Khasra 412/2"]
    D1[/"Document doc_7f3a91c2<br/>p.3 · sha256 9b1c…"/] -- EVIDENCES --> M1
```

**Schema and constraints (Neo4j 5):**

```cypher
CREATE CONSTRAINT parcel_ulpin IF NOT EXISTS FOR (p:Parcel)   REQUIRE p.ulpin IS UNIQUE;
CREATE CONSTRAINT person_id    IF NOT EXISTS FOR (h:Person)   REQUIRE h.person_id IS UNIQUE;
CREATE CONSTRAINT mutation_no  IF NOT EXISTS FOR (m:Mutation) REQUIRE (m.village_code, m.mutation_no) IS UNIQUE;
CREATE CONSTRAINT doc_sha      IF NOT EXISTS FOR (d:Document) REQUIRE d.sha256 IS UNIQUE;
CREATE INDEX owned_interval    IF NOT EXISTS FOR ()-[o:OWNED]-() ON (o.from_date, o.to_date);
```

**Timeline query** (drives the D3.js land-story view):

```cypher
MATCH (p:Parcel {ulpin: $ulpin})<-[:ON_PARCEL]-(m:Mutation)
OPTIONAL MATCH (m)-[:FROM]->(f:Person)
OPTIONAL MATCH (m)-[:TO]->(t:Person)
OPTIONAL MATCH (d:Document)-[:EVIDENCES]->(m)
RETURN m.date AS date, m.type AS type, m.mutation_no AS mutation_no,
       collect(DISTINCT f.name_translit) AS from, collect(DISTINCT t.name_translit) AS to,
       collect(DISTINCT {doc_id: d.doc_id, page: d.page}) AS evidence
ORDER BY date ASC;
```

**Why Neo4j *and* NetworkX?** Neo4j is the persistent system of record for lineage. NetworkX runs in-worker for algorithms on small per-parcel subgraphs (cycle detection, topological sort, share propagation), which avoids round-trips for every check.

---

### 4.5 Evidence-Backed RAG Layer

The LLM does **not** decide whether a conflict exists; the invariants do that. The LLM only **explains** a flag that already exists, in plain language, and it can use only evidence retrieved from the original scans.

**Indexing.** Every extracted field region becomes a chunk that keeps its pixel provenance:

```python
# rag/indexer.py
chunk = {
    "chunk_id": "chk_000418",
    "text": "खाते क्र. 1187 · सर्वे नं. 412/2 · क्षेत्र 1.20 हे.",
    "text_translit": "khate kr. 1187 · sarve nan. 412/2 · kshetra 1.20 he.",
    "doc_id": "doc_7f3a91c2",
    "page": 3,
    "bbox": [412, 1180, 1630, 1342],
    "doc_sha256": "9b1c0e4f5d…a27e",
    "ulpin": "MH27A0B4C9D1E2",
    "record_id": "rec_01J9Z4K7X2B8",
}
```

- **Embeddings:** `intfloat/multilingual-e5-base` (768-d, supports Indian languages), L2-normalised.
- **Index:** FAISS `IndexFlatIP` for district-scale pilots; `IndexIVFPQ` (nlist = 4096, m = 64) beyond ~5 M chunks.
- **Retrieval:** results are first **filtered by ULPIN / village** using metadata, then ranked semantically, so evidence never leaks in from another parcel.

**Citation-locked generation.** A local open-weights instruct model (Qwen2.5-7B-Instruct or Llama-3.1-8B-Instruct, served via vLLM) receives the conflict object plus the top-k chunks. The system prompt requires every sentence to cite a `chunk_id`. A post-generation validator then enforces the contract:

```python
# rag/validator.py
import re

CITE = re.compile(r"\[(chk_\d{6})\]")

def validate(answer: str, retrieved_ids: set[str]) -> str:
    sentences = [s for s in re.split(r"(?<=[.!?।])\s+", answer.strip()) if s]
    for s in sentences:
        cited = set(CITE.findall(s))
        if not cited or not cited <= retrieved_ids:
            return "INSUFFICIENT_EVIDENCE"   # never show an uncited claim to an officer
    return answer
```

Because every `chunk_id` maps to `(doc_id, page, bbox)`, the dashboard draws a **highlight box on the original scan** for each citation. The officer sees the exact handwriting the claim rests on.

---

### 4.6 Cryptographic Trust & Audit Trails

**Two layers:**

1. **Document fingerprinting.** At ingest, the raw file bytes are hashed with SHA-256 (FIPS 180-4) *before any processing*. Every derived record, chunk and conflict carries the source `sha256`, so any later change to a stored document is detectable.
2. **Hash-chained audit ledger.** Every event (ingest, extraction correction, conflict raised, officer decision, sync) is appended to an append-only MongoDB collection. Each event is linked to the one before it:

```math
h_0 = \text{SHA256}(\texttt{"GENESIS"} \,\|\, \text{deployment\_id}), \qquad
h_i = \text{SHA256}\big(h_{i-1} \,\|\, \text{canonical\_json}(e_i)\big)
```

```python
# security/ledger.py
import hashlib, json

def canonical(e: dict) -> bytes:
    return json.dumps(e, sort_keys=True, separators=(",", ":"), ensure_ascii=False).encode()

def append(prev_hash: str, event: dict) -> str:
    return hashlib.sha256(bytes.fromhex(prev_hash) + canonical(event)).hexdigest()

def verify_chain(events: list[dict], genesis: str) -> tuple[bool, int | None]:
    h = genesis
    for i, e in enumerate(events):
        body = {k: v for k, v in e.items() if k != "hash"}
        h = append(h, body)
        if h != e["hash"]:
            return False, i          # first tampered index
    return True, None
```

> [!WARNING]
> **Threat model, stated honestly.** Hashing *detects* tampering; it does not *prevent* it. An insider with database write access could recompute the whole chain. To catch that, the ledger head hash is **checkpointed every hour** to an external write-once store: a signed checkpoint file held by a separate authority (e.g. State Data Centre audit bucket with object lock). Officer decisions are also signed with the officer's DSC key. A rewritten chain will no longer match the last external checkpoint.

---

### 4.7 Human-in-the-Loop Workflow & RBAC

| Action | Patwari | Revenue Inspector | Tehsildar | Auditor | Admin |
|---|:-:|:-:|:-:|:-:|:-:|
| Upload scans | ✅ | ✅ | ✅ | — | — |
| Correct low-confidence fields | ✅ | ✅ | ✅ | — | — |
| Confirm / reject name links | ✅ | ✅ | ✅ | — | — |
| View conflicts & evidence | ✅ | ✅ | ✅ | ✅ | — |
| **Decide on a conflict** | — | Recommend | ✅ | — | — |
| Approve sync to RoR | — | — | ✅ | — | — |
| Verify ledger integrity | — | — | ✅ | ✅ | ✅ |
| Manage users & roles | — | — | — | — | ✅ |

Authentication uses JWT bearer tokens with role claims. Rate limiting (`slowapi`, token bucket per user + IP) protects the ingest and RAG endpoints. Aadhaar-linked fields are masked in every view below Tehsildar, in line with the **DPDP Act, 2023**.

---

## 📁 Repository Structure

```text
bhu-sanvad/
├── api/
│   ├── main.py                    # FastAPI app factory, middleware, routers
│   ├── deps.py                    # auth, DB sessions, RBAC dependencies
│   ├── routers/
│   │   ├── ingest.py              # /api/v1/ingest, /api/v1/jobs/{job_id}
│   │   ├── records.py             # /api/v1/records/{khasra_id}/...
│   │   ├── rag.py                 # /api/v1/rag/explain
│   │   ├── review.py              # /api/v1/review/... (HITL decisions)
│   │   └── verify.py              # /api/v1/verify/integrity
│   └── schemas/                   # Pydantic models (LandRecord, Conflict, ...)
├── models/
│   ├── pipeline/
│   │   ├── preprocess.py          # deskew, denoise, stamp mask, Sauvola
│   │   ├── layout.py              # LayoutLMv3 region classification
│   │   ├── htr_trocr.py           # IndicTrOCR recogniser
│   │   ├── htr_crnn.py            # CRNN + CTC recogniser
│   │   └── agreement.py           # two-model agreement gate
│   ├── extraction/
│   │   ├── fields.py              # region → schema mapping per register format
│   │   ├── units.py               # state-aware area normalisation
│   │   └── names.py               # transliteration + entity resolution
│   └── weights/                   # (git-ignored) downloaded checkpoints
├── graph/
│   ├── builder.py                 # Neo4j upserts
│   ├── invariants.py              # I1–I7
│   ├── risk.py                    # parcel risk score
│   └── cypher/                    # constraints.cypher, timeline.cypher, i5.cypher
├── gis/
│   ├── migrations/                # Alembic migrations for PostGIS
│   └── spatial.py                 # I7 spatial consistency
├── rag/
│   ├── indexer.py                 # chunking + FAISS index build
│   ├── retriever.py               # metadata-filtered retrieval
│   ├── generator.py               # vLLM client + prompt
│   └── validator.py               # citation enforcement
├── security/
│   ├── hashing.py                 # SHA-256 document fingerprints
│   ├── ledger.py                  # hash-chained audit ledger
│   └── checkpoint.py              # external signed checkpoints
├── workers/
│   ├── celery_app.py
│   └── tasks.py                   # preprocess → htr → extract → graph → conflicts → rag
├── frontend/
│   ├── index.html
│   ├── css/
│   └── js/
│       ├── map.js                 # Leaflet cadastral view
│       ├── timeline.js            # D3 land-story timeline
│       └── evidence.js            # bbox overlay on scans
├── config/
│   ├── units/                     # per-state unit conversion YAMLs
│   └── formats/                   # per-state register layout definitions
├── data/
│   └── synthetic/                 # synthetic folios + injected-defect manifests
├── tests/
│   ├── unit/
│   ├── integration/
│   └── eval/                      # accuracy & conflict-detection benchmark harness
├── docker/
│   ├── Dockerfile.api
│   ├── Dockerfile.worker
│   └── docker-compose.yml
├── .env.example
├── requirements.txt
├── Makefile
└── README.md
```

---

## ⚙️ Local Setup & Installation

### Prerequisites

| Tool | Version | Notes |
|---|---|---|
| Python | 3.11+ | |
| Docker + Docker Compose | 24+ | Runs MongoDB, Neo4j, PostGIS, Redis |
| CUDA + PyTorch | CUDA 12.1 / Torch 2.x | Optional; the CPU fallback works but HTR is ~8–10× slower |
| Tesseract | 5.x + Indic traineddata | Optional baseline only |

### 1. Clone and create a virtual environment

```bash
git clone https://github.com/<your-org>/bhu-sanvad.git
cd bhu-sanvad
python3.11 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
# GPU build of torch (skip on CPU-only machines):
pip install torch --index-url https://download.pytorch.org/whl/cu121
```

### 2. Start the data services

```yaml
# docker/docker-compose.yml
services:
  mongo:
    image: mongo:7
    ports: ["27017:27017"]
    volumes: [mongo_data:/data/db]
  neo4j:
    image: neo4j:5
    environment:
      NEO4J_AUTH: neo4j/bhusanvad-dev
    ports: ["7474:7474", "7687:7687"]
    volumes: [neo4j_data:/data]
  postgis:
    image: postgis/postgis:16-3.4
    environment:
      POSTGRES_USER: bhusanvad
      POSTGRES_PASSWORD: bhusanvad-dev
      POSTGRES_DB: cadastral
    ports: ["5432:5432"]
    volumes: [pg_data:/var/lib/postgresql/data]
  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]
volumes: { mongo_data: {}, neo4j_data: {}, pg_data: {} }
```

```bash
docker compose -f docker/docker-compose.yml up -d
```

### 3. Configure the environment

```bash
cp .env.example .env
```

```ini
# .env.example
APP_ENV=development
JWT_SECRET=change-me-to-a-64-char-random-string
MONGO_URI=mongodb://localhost:27017/bhusanvad
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=bhusanvad-dev
POSTGIS_DSN=postgresql+psycopg://bhusanvad:bhusanvad-dev@localhost:5432/cadastral
REDIS_URL=redis://localhost:6379/0
FAISS_INDEX_PATH=./data/index/faiss.index
EMBED_MODEL=intfloat/multilingual-e5-base
LLM_ENDPOINT=http://localhost:8001/v1
LLM_MODEL=Qwen/Qwen2.5-7B-Instruct
HTR_PRIMARY_WEIGHTS=./models/weights/indic_trocr
HTR_FALLBACK_WEIGHTS=./models/weights/crnn_ctc.pt
BHASHINI_API_KEY=
ACCEPT_THRESHOLD=0.90
LEDGER_CHECKPOINT_DIR=./data/checkpoints
```

> [!TIP]
> If `BHASHINI_API_KEY` is empty, transliteration falls back to the offline `indic-transliteration` library.

### 4. Run migrations, graph constraints and seed data

```bash
alembic -c gis/alembic.ini upgrade head          # PostGIS tables: land_records, cadastral_parcels
python -m graph.init_constraints                 # applies graph/cypher/constraints.cypher
python -m data.synthetic.generate --folios 200 --defect-rate 0.3   # synthetic folios with injected conflicts
python -m rag.indexer --rebuild                  # builds the FAISS index
```

### 5. Serve the LLM (optional; needed only for `/rag/explain`)

```bash
pip install vllm
python -m vllm.entrypoints.openai.api_server --model Qwen/Qwen2.5-7B-Instruct --port 8001
```

### 6. Run the API, workers and frontend

```bash
# Terminal 1 — API
uvicorn api.main:app --host 0.0.0.0 --port 8000 --reload

# Terminal 2 — workers (separate queues so GPU and I/O scale independently)
celery -A workers.celery_app worker -Q vision --concurrency=1 -n vision@%h --loglevel=info
celery -A workers.celery_app worker -Q graph,rag --concurrency=4 -n core@%h --loglevel=info

# Terminal 3 — frontend
python -m http.server 5500 --directory frontend
```

Open **http://localhost:5500** for the dashboard and **http://localhost:8000/docs** for interactive OpenAPI docs.

### 7. Run the tests

```bash
pytest tests/unit tests/integration -q
make eval            # runs tests/eval → writes reports/eval.md
```

---

## 🔌 API Reference (Core Endpoints)

Base URL: `/api/v1` · Auth: `Authorization: Bearer <JWT>` · Content type: `application/json` unless noted.

> [!NOTE]
> `khasra_id` is the canonical parcel key `{village_code}:{khasra_no}`, with `/` in the khasra number replaced by `-`. Example: village `272104`, khasra `412/2` → `272104:412-2`. Endpoints also accept a 14-character ULPIN wherever a `khasra_id` is expected.

| Method | Endpoint | Payload | Success | Errors |
|---|---|---|---|---|
| `POST` | `/ingest` | `multipart/form-data`: `file` (PDF/TIFF/JPG), `village_code`, `register_type` | `202 Accepted` → `job_id` | `400` bad format · `413` > 50 MB · `415` unsupported type · `429` rate limit |
| `GET` | `/jobs/{job_id}` | — | `200` stage + progress | `404` |
| `GET` | `/records/{khasra_id}` | — | `200` current `LandRecord` | `404` |
| `GET` | `/records/{khasra_id}/timeline` | — | `200` ordered lineage events | `404` |
| `GET` | `/records/{khasra_id}/conflicts` | query: `status`, `min_risk` | `200` conflicts + risk score | `404` |
| `POST` | `/rag/explain` | `{ "conflict_id": str, "language": "en" \| "hi" \| "mr" \| ... }` | `200` cited explanation | `404` · `422` · `503` LLM unavailable |
| `POST` | `/review/{conflict_id}/decision` | `{ "decision": ..., "remarks": str, "signature": str }` | `201` ledger entry | `403` role · `409` already decided |
| `POST` | `/verify/integrity` | `{ "doc_id": str }` **or** `{ "ledger_range": [int, int] }` | `200` verification result | `404` · `422` |

### Examples

**Ingest a folio**

```bash
curl -X POST http://localhost:8000/api/v1/ingest \
  -H "Authorization: Bearer $TOKEN" \
  -F "file=@data/synthetic/folios/haveli_272104_p003.pdf" \
  -F "village_code=272104" \
  -F "register_type=MUTATION_REGISTER"
```

```json
{
  "job_id": "job_01J9Z6Q1M4TP",
  "doc_id": "doc_7f3a91c2",
  "sha256": "9b1c0e4f5d…a27e",
  "status": "QUEUED",
  "stages": ["preprocess", "htr_layout", "extract", "graph", "conflicts", "rag_index"]
}
```

**Get conflicts for a parcel**

```bash
curl -s http://localhost:8000/api/v1/records/272104:412-2/conflicts?min_risk=1 \
  -H "Authorization: Bearer $TOKEN"
```

```json
{
  "khasra_id": "272104:412-2",
  "ulpin": "MH27A0B4C9D1E2",
  "risk_score": 78,
  "risk_band": "RED",
  "conflicts": [
    {
      "conflict_id": "C-0192",
      "rule": "I5_CHAIN_CONTINUITY",
      "severity": 40,
      "confidence": 0.95,
      "status": "OPEN",
      "detail": { "mutation_no": "M-4410", "date": "2016-02-03", "seller_person_id": "per_5521", "seller_owned_until": "2014-11-20" },
      "evidence_record_ids": ["rec_01J9Z4K7X2B8", "rec_01J9Z5A0QF11"]
    },
    {
      "conflict_id": "C-0193",
      "rule": "I1_AREA_CONSERVATION",
      "severity": 30,
      "confidence": 0.97,
      "status": "OPEN",
      "detail": { "parent_sqm": 12000.0, "children_sqm": 12410.0, "delta_sqm": 410.0, "tolerance_sqm": 120.0 },
      "evidence_record_ids": ["rec_01J9Z4K7X2B8", "rec_01J9Z5B7RR02", "rec_01J9Z5B7RR03"]
    }
  ]
}
```

**Explain a conflict**

```bash
curl -s -X POST http://localhost:8000/api/v1/rag/explain \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"conflict_id": "C-0192", "language": "en"}'
```

```json
{
  "conflict_id": "C-0192",
  "explanation": "Mutation M-4410 dated 03-02-2016 records Suresh Patil as the seller of khasra 412/2 [chk_000431]. However, the 2014 partition entry shows his share of khasra 412 was transferred to khasra 412/1 on 20-11-2014 [chk_000418]. No later record shows him re-acquiring any share in 412/2 [chk_000452].",
  "citations": [
    { "chunk_id": "chk_000431", "doc_id": "doc_a81e03d4", "page": 1, "bbox": [388, 902, 1544, 1010] },
    { "chunk_id": "chk_000418", "doc_id": "doc_7f3a91c2", "page": 3, "bbox": [412, 1180, 1630, 1342] },
    { "chunk_id": "chk_000452", "doc_id": "doc_c02b77e9", "page": 2, "bbox": [301, 640, 1502, 755] }
  ],
  "validator": "PASSED",
  "disclaimer": "Decision support only. Final determination rests with the authorised revenue officer."
}
```

**Verify document integrity**

```bash
curl -s -X POST http://localhost:8000/api/v1/verify/integrity \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"doc_id": "doc_7f3a91c2"}'
```

```json
{
  "doc_id": "doc_7f3a91c2",
  "stored_sha256":   "9b1c0e4f5d…a27e",
  "computed_sha256": "9b1c0e4f5d…a27e",
  "document_intact": true,
  "ledger": { "chain_valid": true, "events_checked": 1842, "last_checkpoint": "2026-09-29T17:00:00Z", "checkpoint_match": true }
}
```

---

## 📊 Benchmarks, Impact & National Alignment

### Comparison

| Dimension | Manual Patwari Workflow | Generic Cloud OCR | **Bhu-Sanvad** |
|---|---|---|---|
| Handwritten Indic registers | Human reading | Weak on cursive, stamps and faded ink | Stamp masking + Sauvola + Indic HTR with a two-model agreement gate |
| Output | Paper notes | Raw text blocks | Schema-valid `LandRecord` with a confidence score per field |
| Ownership history | Assembled by hand across files | None | Automatic temporal lineage graph per parcel |
| Conflict detection | Only when a dispute is filed | None | 7 formal invariants + 0–100 risk score |
| Explainability | Officer memory | None | Every claim cited to page + bounding box |
| Tamper evidence | Physical custody | None | SHA-256 fingerprints + hash-chained ledger + external checkpoints |
| Data residency / DPDP 2023 | ✅ | ❌ Foreign cloud processing | ✅ Fully on-prem / State Data Centre / NIC cloud; open-weights models |
| Legal accountability | Officer | Unclear | Officer decides; system only recommends |

### How we measure

All accuracy claims come from a reproducible harness. Nothing in this README is a hand-typed number.

```bash
make eval    # python -m tests.eval.run --dataset data/synthetic --report reports/eval.md
```

| Metric | Definition | Measured on |
|---|---|---|
| **CER / WER** | Character / word error rate of HTR against ground truth | Field crops from synthetic + consented sample folios |
| **Field-level F1** | Exact-match F1 for `khasra_no`, `khata_no`, `area`, `owners`, `mutation_no` | Full-page extraction |
| **Conflict recall** | Share of injected defects (I1–I7) that were flagged | `data/synthetic` with `--defect-rate 0.3` |
| **False-flag rate** | Flags raised on defect-free records | Clean records in the same set |
| **HITL routing rate** | Share of fields sent to human review by the agreement gate | Full pipeline |
| **Time per folio** | Wall-clock time from ingest to conflicts ready (GPU and CPU reported separately) | Full pipeline |

The synthetic generator (`data/synthetic/generate.py`) renders realistic register pages with **invented names**. It then injects labelled defects (area mismatch, share gaps, back-dated mutations, broken chains, duplicate khasras, polygon mismatch) and writes a manifest, so recall and false-flag rate are measured against known ground truth.

### Alignment with national initiatives

| Initiative | How Bhu-Sanvad fits |
|---|---|
| **DILRMP** (DoLR) | Works as an intelligence layer on DILRMP's computerised RoR, digitised maps and registration data; targets the remaining integration gap |
| **Bhu-Aadhaar / ULPIN** | ULPIN is the primary key for every parcel node, every graph edge and every API lookup |
| **Bhu-Naksha** | Cadastral polygons are loaded into PostGIS for the I7 spatial consistency check |
| **SVAMITVA** | Drone-survey property cards for rural inhabited areas can be ingested as a further evidence source for rural lineage |
| **NGDRS** | Registration deeds feed the lineage graph as `Deed` evidence nodes |
| **BHASHINI** | Transliteration and translation for multilingual records and officer-facing explanations |
| **DPDP Act, 2023** | Purpose limitation, role-based masking, on-prem open-weights models, auditable access |
| **UN SDGs** | SDG 1.4 (secure land rights) · SDG 16 (justice & strong institutions) |

---

## 🗺️ Roadmap

| Phase | Scope | Key deliverables |
|---|---|---|
| **v0.1 — Hackathon MVP** | 1 register format, 1 state, synthetic data | Full pipeline, I1–I7, cited RAG, ledger, dashboard |
| **v0.2 — District Pilot** | 1 district, real folios under a data-sharing MoU | Per-format fine-tuning, officer feedback loop feeding HTR retraining |
| **v0.3 — State Integration** | State SDC / NIC cloud deployment | Read-only DILRMP & Bhu-Naksha connectors, approved-correction sync API, DSC-signed decisions |
| **v1.0 — Multi-State** | Multiple states and scripts | Plug-in register formats via `config/formats/`, per-state unit packs |

**Future scope**

- 📱 **Edge scanning app.** Android capture app for Patwaris with on-device quality checks (blur, skew, glare) before upload.
- ☁️ **State SDC / NIC cloud integration.** Helm charts for State Data Centre Kubernetes and GPU node pools for HTR.
- 🧭 **Automated cadastral boundary realignment.** Suggest corrections to polygon vertices when the recorded area and the map area disagree consistently; officer approval is required.
- 🔗 **e-Courts linkage.** Mark parcels with pending litigation, as DILRMP 3.0 plans.
- 🧠 **Active learning.** Every field an officer corrects becomes a labelled HTR training sample.

---

## 🤝 Contribution Guidelines

### Git workflow

- `main` is always deployable and protected. Changes go in through PRs only, with ≥ 1 review.
- `dev` is the integration branch.
- Feature branches come off `dev`:

| Prefix | Use | Example |
|---|---|---|
| `feat/` | New capability | `feat/i7-spatial-check` |
| `fix/` | Bug fix | `fix/sauvola-window-overflow` |
| `docs/` | Documentation | `docs/api-examples` |
| `refactor/` | No behaviour change | `refactor/ledger-canonical-json` |
| `test/` | Tests / eval only | `test/injected-defect-cases` |

**Commits** follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
feat(graph): add I5 chain-of-title continuity invariant
fix(htr): route zero-CER-mismatch digit fields to HITL
```

### PR checklist

- [ ] Branch is up to date with `dev`
- [ ] `pytest tests/unit tests/integration` passes
- [ ] `ruff check .` and `black --check .` pass
- [ ] New invariants include unit tests with **both** a passing and a failing record
- [ ] If the change touches HTR/extraction: `make eval` has been run and the `reports/eval.md` diff is attached to the PR
- [ ] No real citizen data, names, Aadhaar or ULPINs in code, fixtures or screenshots
- [ ] API changes are reflected in Pydantic schemas and in this README's API table
- [ ] Ledger format changes include a migration and keep old chains verifiable

---

## 👥 Team & License

**TEAM MATRIX123** · Team ID `183252` · Smart India Hackathon 2026 · Problem Statement **SIH26018**

Built for the Department of Land Resources, Ministry of Rural Development, Government of India.

Released under the [MIT License](LICENSE).

<div align="center">

*Bhu-Sanvad — a dialogue with the land's own records.*

</div>
