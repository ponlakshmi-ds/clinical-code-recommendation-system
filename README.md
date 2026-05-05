# AI-Assisted Clinical Code List Recommendation System

## Overview

This project explores how AI-assisted retrieval systems can support the development of **clinical code lists** used in healthcare research, population identification, and evidence generation.

Developed as part of an **employer-linked applied project with the National Institute for Health and Care Excellence (NICE)** using publicly available terminology and clinical reference datasets, the project focused on improving how researchers identify clinically relevant codes across fragmented terminology systems.

Clinical code list development is traditionally a **manual, time-intensive, and specialist-driven process**, often requiring analysts to search across hundreds of thousands of clinical concepts distributed across multiple repositories. This introduces challenges around consistency, reproducibility, and review effort.

To address this, the project designed and developed an **AI-assisted recommendation workflow** that combines:

* curated codelist discovery
* semantic similarity search
* ontology hierarchy traversal
* benchmark-based validation
* LLM-assisted clinical justification

The result is a **deterministic, explainable, and confidence-ranked recommendation pipeline** that supports specialist review while improving efficiency and retrieval coverage.

---

## Business Problem

Healthcare researchers frequently need to define clinical cohorts using standardised code lists.

For example:

> *Identify all relevant codes associated with Type 2 Diabetes for cohort selection and evidence generation.*

This task is challenging because:

* terminology systems contain **hundreds of thousands of concepts**
* clinically relevant concepts may be distributed across multiple sources
* synonyms and coding variation create recall challenges
* code definitions may differ across studies
* manual search and validation is slow and resource-intensive

The objective of this project was to design a workflow that improves:

✅ retrieval efficiency

✅ consistency of code definitions

✅ explainability of recommendations

✅ auditability for specialist review

✅ scalability for future terminology expansion

---

## System Architecture

![Clinical Code Recommendation Workflow](architecture/clinical_code_architecture.png)

The final solution uses a **multi-stage hybrid retrieval architecture**.

### Phase 1 — Query Understanding & Codelist Discovery

A free-text clinical research query is processed into structured search terms.

Candidate codelists are then identified using multiple retrieval approaches:

* TF-IDF similarity search
* SapBERT embeddings
* MiniLM embeddings

Primary source used:

* OpenCodelists

This phase prioritises clinically meaningful grouped code lists rather than isolated concept retrieval.

---

### Phase 2 — Semantic & Hierarchical Clinical Retrieval

A SNOMED CT master dictionary was built and enriched using:

* synonym mapping
* parent concept relationships ("IS-A")
* hierarchy traversal structures

Two retrieval paths operate independently:

#### Semantic Search

Uses **SapBERT embeddings + FAISS vector search** to retrieve clinically similar concepts.

#### Hierarchical Expansion

Uses **Lowest Common Ancestor (LCA) traversal** to identify structurally related parent / child concepts within the clinical ontology.

This dual-path approach improves both semantic relevance and structural clinical coverage.

---

### Phase 3 — Validation & Confidence Scoring

Retrieved concepts are validated against benchmark clinical sources:

* NHS reference sets
* QOF clinical business rules

Evidence from multiple retrieval paths is combined into confidence tiers:

### High Confidence

Agreement across multiple evidence paths

### Medium Confidence

Partial evidence agreement

### Low Confidence

Single-path evidence requiring manual review

This creates an explainable prioritisation layer for specialist validation.

---

### Phase 4 — LLM-Assisted Clinical Justification

A justification layer was implemented using **Google Gemini 2.5 Flash-Lite**.

Prompting strategies explored:

* deterministic rule-based prompting
* zero-shot prompting

The LLM layer provides:

* inclusion reasoning
* exclusion reasoning
* confidence explanation
* clinical interpretation support

This separates **retrieval evidence** from **clinical explanation**, improving transparency and auditability.

---

## Final Output

The system generates a structured Excel output containing:

* full recommended code list
* source evidence columns for each retrieval / validation phase
* justification / reasoning for inclusion
* confidence classification

Output tabs include:

1. Full recommendation list
2. High Confidence
3. Medium Confidence
4. Low Confidence

This allows researchers to prioritise review efficiently.

---

## My Contribution

My primary contributions in this project included:

### Project Coordination & Delivery

* coordinated team workstreams across data, modelling, and evaluation components
* supported architecture alignment and design decision-making
* facilitated structured development planning and cross-functional collaboration

### Clinical Knowledge Integration

* designed and built the enriched **SNOMED CT master dictionary**
* integrated synonym expansion and hierarchy preparation for retrieval workflows
* supported preparation of ontology structures for semantic and hierarchical search

### Validation & Confidence Framework

* designed validation logic using **QOF rules** and **NHS reference sets**
* contributed to confidence-tier scoring methodology
* supported creation of explainable evidence pathways for specialist review

---

## Technologies Used

* Python
* NumPy
* Pandas
* TF-IDF
* SapBERT embeddings
* MiniLM embeddings
* FAISS vector similarity search
* SNOMED CT ontology hierarchy
* Lowest Common Ancestor traversal
* Google Gemini 2.5 Flash-Lite
* Excel-based structured recommendation outputs

---

## Key Learnings

This project strengthened my understanding of:

* retrieval-augmented healthcare AI workflows
* vector search and embedding-based semantic retrieval
* ontology-aware recommendation systems
* explainable AI in regulated environments
* benchmark validation design
* balancing recall, precision, and human review effort
* designing deterministic AI systems where reproducibility matters

One of the most important project learnings was recognising that **high-performing AI systems in healthcare must prioritise explainability, auditability, and reproducibility — not just automation**.

---

## Future Extensions

Potential future enhancements include:

* integration of ICD-10 and OPCS-4 terminology systems
* persistent vector knowledge base implementation (e.g., Chroma)
* expanded synonym and concept linking coverage
* richer multi-condition comorbidity mapping
* deployment as an interactive research support tool for analyst review workflows
