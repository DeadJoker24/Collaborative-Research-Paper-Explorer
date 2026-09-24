# IT314 – Software Engineering | Lab Assignment 6

# Requirement Formalization & Process Modelling

**Group-19**

**Project Name:** Collaborative Research Paper Explorer  
**Development Model:** Agile (SCRUM)

---

## Summary

The Collaborative Research Paper Explorer is designed to simplify and speed up how researchers and students read, analyze, and discuss scientific literature.

It provides an interactive reading workspace that allows users to:

- Review papers side-by-side with AI-generated summaries.
- Extract key figures and tables.
- Create technical notes with mathematical notation.
- Explore visual citation graphs.
- Search and organize research papers using hierarchical topic tags.
- Collaboratively annotate and discuss papers.

Following an iterative Agile requirement elicitation process, feedback from all 7 key stakeholders was gathered using interviews, observations, document analysis, and brainstorming sessions.

This resulted in a verified and confirmed set of:

- Functional Requirements
- Non-Functional Requirements
- Domain Requirements

---

# 2.1 Stakeholders & Elicitation Formalization

All requirement elicitation cycles across the 7 project stakeholders have been completed and formally signed off.

## 1. Researcher — Primary End User

**Role:**

- Uploads papers.
- Studies proofs.
- Annotates text with LaTeX.
- Explores literature networks.

**Technique:** Interview

**Justification:**

Semi-structured interviews allow deep exploration of literature review bottlenecks, annotation pain points, and trust concerns regarding GenAI hallucinations.

**Status:** COMPLETED

### Findings Gathered

- Manual skimming of 30–50 page papers is highly inefficient.
- Requires side-by-side reading layout with AI summaries linked directly to paragraph coordinates.
- LaTeX math typesetting and Markdown annotation support are mandatory.
- Requires hierarchical topic tagging with AI-assisted tag deduplication when assigning new labels to papers.

**Closure Status:** Completed & Confirmed.

The researcher interviews were summarized and requested features were finalized into **FR-06 to FR-15**.

---

## 2. Student — Secondary End User

**Role:**

- Conducts literature reviews for coursework/theses.
- Learns unfamiliar topics.
- Builds reading lists.

**Technique:** Observation

**Justification:**

Direct observation uncovers unarticulated search struggles, how students navigate unfamiliar academic jargon, and their reading flow.

**Status:** COMPLETED

### Findings Gathered

- Students frequently get stuck on advanced terminology without guided topic hierarchies.
- Students need to filter papers by specific sub-topics using hierarchical tags without creating fragmented synonym tags.
- Students want beginner-friendly summaries.
- Students want an interactive visual citation graph to discover foundational papers.

**Closure Status:** Completed & Confirmed.

These findings were approved into:

- FR-10
- FR-12
- FR-13
- FR-16

---

## 3. System Admin — Operational & Security Custodian

**Role:**

- Maintains infrastructure uptime.
- Enforces RBAC.
- Monitors LLM API costs.
- Oversees user management.

**Technique:** Interview

**Justification:**

Structured interviews with campus system administrators are critical for defining security policies, session timeout thresholds, compute quotas, and administrative monitoring dashboards.

**Status:** COMPLETED

### Findings Gathered

- Role-Based Access Control (RBAC) is required:
  - Admin
  - Annotator
  - Viewer
- Upload ceiling is 30 MB / 50 pages.
- Automatic session expiration after 30 minutes of inactivity.
- Admin dashboard must track:
  - Storage quotas
  - Redis queues
  - LLM token expenditures

**Closure Status:** Completed & Confirmed.

The rules were finalized into:

- FR-01 to FR-05
- FR-21

---

## 4. Research Paper Archive Servers

**Examples:**

- arXiv
- bioRxiv
- Open-Access Repositories

**Role:**

External repositories supply direct open-access PDFs and preprints.

**Technique:** Document Analysis

**Sources:**

- Official arXiv API documentation
- OAI-PMH harvesting specifications

**Justification:**

Rate-limiting policies, query schemas, and data formats must be derived from official manuals.

**Status:** COMPLETED

### Findings Gathered

- arXiv provides bulk metadata via OAI-PMH.
- Direct PDF streaming is available through unique paper IDs.
- Raw LaTeX source code enables high-accuracy equation parsing.
- API query rate limit is maximum 1 request per 3 seconds.

**Closure Status:** Completed & Confirmed.

These rules were locked into:

- DR-02
- DR-05

---

## 5. GenAI API — External LLM & Embedding Service

**Role:**

- Generates structured section summaries.
- Performs Agentic AI tag deduplication.
- Recommends literature.

**Technique:** Document Analysis

**Sources:**

- Provider API reference manuals.
- Context limits.
- Token pricing documentation.

**Justification:**

Model context-window capacities, latency characteristics, embedding dimensions, and financial costs are rigid technical specifications.

**Status:** COMPLETED

### Findings Gathered

- Context-window constraints require PDF text chunking before summarization.
- Asynchronous worker queues using Celery/Redis must decouple slow LLM generation from the client UI.
- SHA-256 document hashing avoids re-processing identical papers.
- Agentic AI workflows can perform semantic similarity evaluation to identify synonymous or duplicate tags.

**Closure Status:** Completed & Confirmed.

Finalized requirements:

- FR-08
- FR-12
- NFR-03

---

## 6. Journal & Licensing Institutes — Publishers & Campus Copyright Authorities

**Role:**

Governs:

- Fair-use boundaries.
- Library subscriptions.
- Open Access rights.
- Institutional data policies.

**Technique:** Document Analysis

**Sources:**

- Campus digital resource agreements.
- Publisher terms.

**Status:** COMPLETED

### Findings Gathered

- Open-Access/arXiv papers permit automated text mining for non-commercial analysis.
- Proprietary publisher PDFs cannot be publicly hosted or indexed.
- Display must be restricted to authenticated private workspace collaborators.
- Permanent data deletion must be supported upon verified user request.

**Closure Status:** Completed & Confirmed.

Finalized requirements:

- DR-01
- DR-06
- NFR-13

---

## 7. System Database — Internal Core Data Architecture

**Role:**

Persists:

- User authentication records.
- Hierarchical topic tag ontologies.
- Section coordinate anchors.
- Annotations.

**Technique:** Brainstorming

**Justification:**

Engineering brainstorming evaluates database paradigms such as:

- PostgreSQL relational schema + pgvector.
- Graph databases.

It also helps design hierarchical tag data structures.

**Status:** COMPLETED

### Findings Gathered

- Dedicated normalized tables are required.
- Tags must be linked to coordinate bounding boxes.
- Full-text search indices using GIN are required.
- Vector embeddings using HNSW are required.
- Search should support sub-500ms multi-tag queries.
- ACID compliance is mandatory for collaborative annotations and user permissions.

**Closure Status:** Completed & Confirmed.

Database design was finalized into:

- FR-10
- FR-11
- NFR-09

---

# 2.2 Consolidated Requirements Documentation

Every requirement possesses:

- Unique identifier.
- Stakeholder traceability.
- MoSCoW priority.
- Confirmed validation status.

---

# 1. Functional Requirements (FRs)

| ID | Requirement | Source Stakeholder(s) | Priority | Status |
|---|---|---|---|---|
| **FR-01** | The system shall allow new users to register with an institutional email, password, full name, affiliation, and primary research interests. | Researcher, Student, System Admin | HIGH | CONFIRMED |
| **FR-02** | The system shall support secure user authentication via email/password and Institutional Single Sign-On (SSO / Google Edu / SAML). | Researcher, Student, System Admin | HIGH | CONFIRMED |
| **FR-03** | The system shall provide a self-service password reset flow via a time-limited 15-minute cryptographically secure email link. | Researcher, Student | HIGH | CONFIRMED |
| **FR-04** | The system shall provide an explicit logout function and automatically terminate sessions after 30 minutes of inactivity, invalidating tokens. | System Admin | HIGH | CONFIRMED |
| **FR-05** | The system shall enforce Role-Based Access Control (RBAC) across System Admin, Lab Annotator, and Read-Only Viewer tiers. | System Admin | HIGH | CONFIRMED |
| **FR-06** | The system shall allow uploading research papers in PDF format up to 30 MB and direct importing from Archive Servers via paper IDs such as arXiv ID. | Researcher, Student, Archive Servers | HIGH | CONFIRMED |
| **FR-07** | The system shall automatically extract document metadata including Title, Authors, Abstract, Year, and DOI and store it in the system database. | Researcher, System Database | HIGH | CONFIRMED |
| **FR-08** | The system shall auto-generate structured section summaries including Background, Methodology, Results, and Limitations using the GenAI API, linked to source coordinates. | Researcher, GenAI API | HIGH | CONFIRMED |
| **FR-09** | The system shall detect and crop key figures, charts, and tables from PDFs and catalog them in an interactive asset viewer. | Researcher, Student, GenAI API | HIGH | CONFIRMED |
| **FR-10** | The system shall maintain a dedicated database for hierarchical topic tags supporting nested paths such as `green_energy` and `green_energy::solar_panels`. | Researcher, Student, System Database | HIGH | CONFIRMED |
| **FR-11** | The system shall allow users and GenAI to assign hierarchical tags to specific section spans or coordinate bounding boxes within a paper. | Researcher, GenAI API, System DB | HIGH | CONFIRMED |
| **FR-12** | The system shall use Agentic AI with the Tag System database to detect duplicate or equivalent tags when a researcher or student creates a new tag, prompting consolidation. | Researcher, Student, GenAI API, System Database | HIGH | CONFIRMED |
| **FR-13** | The system shall allow users to search, filter, and discover papers across personal libraries and global indexes using hierarchical topic tags. | Researcher, Student, System Database | HIGH | CONFIRMED |
| **FR-14** | The system shall utilize section-level hierarchical topic tags and text embeddings to generate ranked AI recommendations of related literature. | Student, Researcher, GenAI API | HIGH | CONFIRMED |
| **FR-15** | The system shall provide a split-pane reader displaying the original PDF on the left and rich annotations using LaTeX/Markdown on the right. | Researcher, Student | HIGH | CONFIRMED |
| **FR-16** | The system shall render an interactive citation graph showing backward references, forward citations, and co-citation clusters. | Researcher, Student, Archive Servers | HIGH | CONFIRMED |
| **FR-17** | The system shall support threaded contextual discussions anchored to text highlights or extracted figures with multi-user @mention tagging. | Researcher, Student | HIGH | CONFIRMED |
| **FR-18** | The system shall allow researchers to invite collaborators to papers/collections with granular permission tiers: View, Comment, Edit. | Researcher, System Admin | HIGH | CONFIRMED |
| **FR-19** | The system shall dispatch real-time notifications when a user is mentioned or when notes/tags are added to a followed paper. | Researcher, Student | MEDIUM | CONFIRMED |
| **FR-20** | Users shall be able to export annotations, summaries, section tags, and citations as formatted BibTeX, Markdown, or annotated PDFs. | Researcher, Student | MEDIUM | CONFIRMED |
| **FR-21** | System administrators shall have a dashboard to track storage quotas, Redis job queues, and GenAI API token consumption/costs. | System Admin, GenAI API | HIGH | CONFIRMED |

---

# 2. Non-Functional Requirements (NFRs)

| ID | Quality Attribute | Requirement Specification | Target Metric | Priority |
|---|---|---|---|---|
| **NFR-01** | Auth Latency | User login and token validation must execute with minimal latency. | ≤ 500 ms round-trip time | HIGH |
| **NFR-02** | PDF Rendering | Client-side PDF rendering engine must load and render the initial page viewport of a 50-page paper within responsive limits. | ≤ 1.5s on 10 Mbps connection | HIGH |
| **NFR-03** | Throughput | GenAI summarization, figure extraction, and related-work pipelines must complete without UI timeouts. | ≤ 30s for a 20-page PDF for 90% of requests | HIGH |
| **NFR-04** | Tag Search Latency | Hierarchical tag queries must return filtered paper results instantly. | ≤ 300 ms across 100,000 indexed records | HIGH |
| **NFR-05** | Tag Deduplication Latency | Agentic AI semantic evaluation during tag creation must return duplication warnings smoothly. | ≤ 1.2s per tag evaluation call | HIGH |
| **NFR-06** | Concurrency | Backend must support concurrent active user sessions without severe response degradation. | ≥ 500 active users; degradation ≤ 2× baseline | MEDIUM |
| **NFR-07** | Fault Tolerance | If GenAI processing fails or times out, system shall retry up to 2 times and inform the user without losing the uploaded file. | Up to 2 retries; ≥ 99.5% error handling | HIGH |
| **NFR-08** | Visualization | Dynamic citation graph must support smooth navigation, zooming, and node dragging. | ≥ 45 FPS with up to 250 connected nodes | MEDIUM |
| **NFR-09** | Data Integrity | Database schema must maintain foreign key integrity across nested hierarchical tags and paper section coordinate anchors. | ACID compliant; zero orphaned tags/anchors | HIGH |
| **NFR-10** | Cost Efficiency | System must cache and batch GenAI requests so previously processed papers are not re-summarized. | 100% cache hit on identical SHA-256 hashes | MEDIUM |
| **NFR-11** | Security | Credentials, session tokens, annotations, and uploaded papers must be encrypted in transit and at rest. | TLS 1.3 in transit; AES-256 at rest | HIGH |
| **NFR-12** | Availability | Core platform services must maintain continuous operational uptime. | ≥ 99.0% uptime excluding maintenance | MEDIUM |
| **NFR-13** | Data Governance | System shall allow users to permanently delete account data and uploaded papers upon verified request. | Complete purge from S3/DB within 24 hours | HIGH |

---

# 3. Domain Requirements (DRs)

## DR-01: Fair Use & Academic Copyright Boundaries

Uploaded research papers in PDF format shall not be publicly redistributed or crawled by third-party search engines in full.

Document access and annotation displays must be restricted to the uploading researcher and explicitly authorized workspace collaborators.

---

## DR-02: Bibliographic Schema Standardization & Interoperability

Citation, reference, and metadata parsing must conform to recognized international bibliographic standards.

Extracted Digital Object Identifiers (DOIs) must validate against the central `doi.org` registry for interoperability with standard reference managers.

---

## DR-03: Hierarchical Scientific Ontology Taxonomy

The database taxonomy for hierarchical topic tags must accommodate multi-tier scientific classifications, such as:

- ACM Computing Classification System
- MeSH
- arXiv taxonomy

The system must allow consistent child-parent inheritance.

---

## DR-04: Mathematical Formula & Scientific Notation Fidelity

The document parsing and annotation rendering engines must preserve mathematical equations and scientific notation without syntax degradation.

They must conform to:

- MathML
- AMS-LaTeX rendering specifications

---

## DR-05: Archive Server Rate Limits & Open-Access Attribution

Integrations with open-access repositories such as arXiv and bioRxiv must:

- Respect OAI-PMH harvesting protocols.
- Enforce a maximum rate of 1 request per 3 seconds.
- Provide clear open-access attribution statements.

---

## DR-06: Institutional Data Retention & Repository Governance

Storage duration, archiving policies, and deletion protocols for user-uploaded papers must align with host institution digital library policies.

Paper metadata must remain exchangeable with institutional repositories using Dublin Core schemas.

---

## DR-07: Scholarly Layout & Coordinate Bounding Conventions

The PDF extraction pipeline must reliably interpret standard scholarly publishing layouts including:

- Multi-column flows.
- Vector diagrams.
- Embedded tables.

Annotations and topic tags must be mapped to normalized coordinate bounding boxes:

```text
(x, y, w, h)
