<!-- markdownlint-disable -->

# Phase 0 – Enrichment Output Integration Boundary (Downstream Consumer Contract)

**Project:** Truly Madly Deeply Ltd
**Programme:** Phase 0 UK E-commerce
**Document ID:** PH0-AI-ENRICH-BOUNDARY
**Status:** AUTHORITATIVE – SUBJECT TO MASTER ACCEPTANCE
**Version:** 1.0
**Applies From:** Phase 0 onwards unless superseded by Master approval

**Binding Inputs (Read-only):**

* Phase 0 – Global AI Enrichment Principles & Governance (Chunk 2A) 
* Phase 0 – AI Enrichment Run Model (Chunk 2B-1) 
* Phase 0 – Prompt Discipline & Canonical Enrichment Prompt (Chunk 2B-2) 
* Phase 0 – Canonical Enrichment Output Contract (Chunk 2B-3) 
* Phase 0 – Manual Enrichment Execution & Operator Workflow (Chunk 2B-4) 

---

## 1. Purpose and Authority

This document defines and locks the **integration boundary** between:

1. **Phase 0 enrichment outputs** produced under the governed enrichment process, and
2. **All downstream consumers**, whether manual or automated, that ingest enrichment outputs for any purpose.

This is a **boundary-locking document**. It constrains downstream behaviour so that:

* Enrichment outputs are treated as **immutable inputs**.
* Downstream systems **do not reinterpret, enrich, infer, or improve** enrichment data.
* Enrichment changes (E000x / P000x) **do not force redesign** of pages, navigation, URLs, templates, or sitemap logic.
* Manual execution today and automation tomorrow **behave identically** at the integration boundary.

---

## 2. Boundary Definition

### 2.1 What Constitutes “Enrichment Output”

An enrichment output is an **accepted** JSON artefact that:

* Conforms to **OC0001 v1.0** (and any later Master-approved contract versions), and
* Is produced under a specific `enrichment_version` (E000x) and `prompt_id` (P000x), and
* Is stored and retained as defined by the Phase 0 operator workflow.

Only **accepted** outputs are eligible for downstream ingestion.

### 2.2 Immutable Input Rule (Absolute)

For all downstream consumers, an accepted enrichment output is **read-only** and **immutable**.

Downstream consumers MUST treat each accepted output as:

* The **single source of truth** for all fields it contains, and
* The only permitted input for building any downstream artefact that depends on enrichment.

---

## 3. What Downstream Consumers MAY Do

Downstream consumers MAY:

1. **Read and validate** enrichment outputs strictly against the published output contract (OC0001) prior to use.
2. **Select** which accepted outputs to ingest based on explicit criteria outside enrichment (for example, operational inclusion lists), without altering the output content.
3. **Render and present** values exactly as provided, including:

   * using the value payloads as-is, and
   * respecting the `ValueWithProvenance` structure and the explicit `provenance` markings.
4. **Format** presentation only where it is mechanically required by the target output medium, provided the underlying value is unchanged (for example, escaping for HTML output, JSON parsing, numeric formatting for display that does not alter the stored value).
5. **Fail fast** and stop processing when encountering:

   * invalid JSON,
   * contract non-compliance,
   * missing required contract fields,
   * type mismatches,
   * unsupported contract identifiers or versions.

Downstream consumers MUST implement deterministic behaviour. Identical accepted inputs MUST produce identical downstream artefacts, subject only to deterministic build tooling.

---

## 4. What Downstream Consumers MUST NOT Do

Downstream consumers MUST NOT:

1. **Modify** enrichment outputs, including:

   * editing, rewriting, trimming, normalising, or “fixing” any field,
   * adding or removing fields,
   * reordering content for non-mechanical reasons,
   * merging fields, splitting fields, or coercing types beyond contract parsing.

2. **Reinterpret** enrichment outputs, including:

   * inferring missing values,
   * substituting defaults,
   * applying business rules that change meaning,
   * altering classification, tags, titles, descriptions, pricing fields, or availability fields.

3. **Enrich** enrichment outputs, including:

   * generating additional copy,
   * generating new SEO fields,
   * generating additional keywords or tags,
   * generating new category paths or navigation hints,
   * creating any new “improved” variants of the enriched content.

4. **Override** enriched values per product, including:

   * manual edits,
   * per-category tweaks,
   * supplier-specific tweaks,
   * “hotfix” patches.

5. **Treat provenance as advisory.** Provenance is part of the contract, and MUST NOT be recalculated or rewritten downstream.

6. **Create coupling** between enrichment versions and page/template structure, including:

   * branching page/template logic by `enrichment_version` or `prompt_id`,
   * requiring different template shapes for different enrichment versions,
   * introducing any downstream rule that makes E000x changes trigger redesign.

Any violation of this section constitutes a governance breach and MUST be escalated to the Master.

---

## 5. Enrichment → Site Build Hand-off Responsibilities

### 5.1 Responsibilities of Enrichment (Upstream)

The enrichment process is responsible for producing accepted outputs that:

* Fully comply with the output contract (OC0001),
* Contain all required fields as defined by the contract,
* Explicitly mark provenance for governed fields,
* Are versioned by `enrichment_version` and `prompt_id`,
* Are retained immutably for audit and rollback.

### 5.2 Responsibilities of Downstream Consumers

Downstream consumers are responsible for:

1. **Contract validation** prior to ingestion.
2. **Deterministic ingestion** that does not alter meaning or structure of the enrichment output.
3. **Deterministic rendering** where output artefacts are generated (pages, feeds, indexes, sitemaps), without adding enrichment logic.
4. **Hard failure behaviour** when contract compliance is not met, including stopping processing and raising an escalation.

Downstream consumers are not responsible for fixing enrichment outputs. All defects in enrichment outputs are addressed only through governed change control (new versions and regeneration).

---

## 6. Protection Against Knock-on Redesign

### 6.1 No Redesign Trigger Rule (Absolute)

Changes to enrichment logic (E000x) or prompt text (P000x) MUST NOT:

* force page template redesign,
* force navigation redesign,
* force URL structure changes,
* force page scaffolding changes,
* force sitemap logic changes.

Downstream consumers MUST be designed and operated such that:

* They accept any valid enrichment output that conforms to the locked contract, without redesign, and
* They do not embed assumptions that only hold for a specific enrichment version.

### 6.2 Contract Change Handling

If a change is proposed that would require downstream redesign, it is **out of scope for Phase 0** unless explicitly approved by the Master with a versioned amendment and an explicit migration instruction set.

Absent such approval, downstream consumers MUST treat any breaking change as invalid and MUST hard-fail and escalate.

---

## 7. Automation Equivalence Rule

All downstream consumer behaviour MUST be identical whether performed manually or via automation.

This means:

* Manual consumers MUST follow the same read-only rules, validation rules, and failure rules as automated consumers.
* Automation MUST NOT introduce behaviour that manual operation is not permitted to perform.
* No downstream step may rely on human judgement to “fix” enrichment outputs.

---

## 8. Failure and Escalation Boundary

If a downstream consumer cannot proceed without violating this boundary contract, it MUST:

1. Stop immediately, and
2. Escalate to the Master with:

   * the affected `run_id`, `enrichment_version`, and `prompt_id`,
   * the specific contract violation or incompatibility,
   * the precise downstream step that would otherwise require reinterpretation or modification.

Local workarounds are forbidden.

---

## 9. Completion & Handover Statement (Formal)

**To: Master – ORA Programme**

This document formally completes **Chunk 2B-5 – Enrichment Output Integration Boundary (Downstream Consumer Contract)**.

The following are confirmed:

1. The integration boundary between enrichment outputs and downstream consumers is explicitly defined and enforceable.
2. Accepted enrichment outputs are locked as immutable inputs downstream.
3. Downstream consumers are prohibited from reinterpretation, enrichment, inference, improvement, or per-product correction.
4. Enrichment version changes (E000x / P000x) are explicitly prevented from triggering redesign of templates, navigation, URLs, or sitemap logic.
5. Manual operation and future automation are constrained to identical behaviour at the boundary.
6. Failure handling is stop-first and escalation-only.

On this basis:

* **Chunk 2B-5 is COMPLETE**
* **Subject to Master acceptance, this Chunk may be locked**
* **No further integration-boundary ambiguity remains**

**End of Chunk 2B-5.**
