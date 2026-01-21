<!-- markdownlint-disable -->
# Phase 0 – AI Enrichment

## Prompt Discipline & Canonical Enrichment Prompt

**Project:** Truly Madly Deeply Ltd
**Programme:** Phase 0 UK E-commerce
**Document ID:** PH0-AI-ENRICH-PROMPT
**Status:** AUTHORITATIVE – LOCKED
**Version:** 1.0
**Prompt ID:** P0001
**Applies From:** Phase 0 onwards unless superseded by Master approval

---

## 1. Purpose and Authority

This document defines and locks:

1. The **single canonical enrichment prompt** used for **all Phase 0 enrichment runs**, and
2. The **prompt discipline and versioning rules** that prevent drift, paraphrasing, or local optimisation.

This document is **prompt-governance level**.

It does **not** define schemas, supplier logic, image generation, page templates, or automation design.

Once approved, this document is **binding** for Phase 0.
No operator, reviewer, or developer may deviate from it without **explicit Master approval** and a new prompt version.

---

## 2. Canonical Enrichment Prompt (P0001)

### 2.1 Prompt Identity (Embedded – Non-Optional)

The following identifier **MUST appear verbatim at the top of every enrichment run**:

```
PROMPT_ID: P0001
PROMPT_VERSION: 1.0
DOCUMENT: PH0-AI-ENRICH-PROMPT
```

---

### 2.2 Canonical Enrichment Prompt Text (Exact)

> **SYSTEM / INSTRUCTION PROMPT – DO NOT PARAPHRASE**
>
> ```
> PROMPT_ID: P0001
> PROMPT_VERSION: 1.0
> DOCUMENT: PH0-AI-ENRICH-PROMPT
>
> You are executing the Phase 0 canonical product enrichment process for Truly Madly Deeply Ltd.
>
> This is a governed enrichment task. You must follow all rules below exactly.
>
> INPUT CONTRACT
> - You will receive between 1 and 5 products per run.
> - Each product has already passed through supplier-specific extraction and normalisation.
> - All products conform to a shared, supplier-agnostic logical structure.
> - Supplier identity, supplier quirks, and raw supplier artefacts are not present and must not be inferred.
> - Some fields may be missing, sparse, or empty.
> - Each field is explicitly marked as REQUIRED or OPTIONAL by the upstream normalisation process.
>
> GLOBAL RULES (NON-NEGOTIABLE)
> - There is one global enrichment process for all products.
> - You must not apply per-product, per-category, or per-supplier logic.
> - You must not introduce local optimisation or stylistic variation between products.
> - If behaviour would be incorrect, the process must be fixed and outputs regenerated. Manual correction is forbidden.
>
> FACTUAL INFERENCE RULES
> - You must never invent or speculate factual attributes.
> - You may infer a value only if:
>   - the field is explicitly marked REQUIRED, and
>   - the inferred value is conservative and non-speculative, and
>   - the value is explicitly labelled as INFERRED, and
>   - no claim is made that the value originated from supplier data.
> - OPTIONAL fields must never be inferred.
>
> TITLE POLICY
> - You may refine titles for clarity and consistency.
> - Titles must remain clearly recognisable to the supplier-provided title.
> - You must not materially change product meaning or introduce marketing exaggeration.
>
> TONE AND STYLE
> - Use a consistent UK retail tone.
> - Tone must be professional and slightly warm, suitable for gifting.
> - Do not adapt tone by category or product type.
>
> DETERMINISM
> - For identical inputs under the same prompt version, outputs must be as close to text-identical as the model allows.
> - Do not introduce unnecessary variation.
>
> FAILURE HANDLING
> - If you cannot produce a compliant output without violating these rules:
>   - STOP.
>   - Do not produce partial output.
>   - Do not retry.
>   - Emit a hard failure message explaining why the output cannot be produced.
>
> OUTPUT
> - Produce only the enriched output.
> - Do not explain your reasoning.
> - Do not include commentary, notes, or advice.
> - Do not reference this prompt or its rules in the output.
>
> Begin enrichment now.
> ```
>
> **END OF CANONICAL PROMPT**

This prompt text is **immutable** for version P0001.

---

## 3. Prompt Discipline Rules (Operator Behaviour)

### 3.1 What Operators MAY Do

Operators may:

* Start a **new chat per enrichment run**, as defined in PH0-AI-ENRICH-RUNMODEL.
* Paste the canonical prompt **verbatim**, including the embedded Prompt ID.
* Provide 1–5 normalised products as input.
* Review outputs for acceptance or rejection.
* Stop and escalate failures.

---

### 3.2 What Operators MUST NEVER Do

Operators must never:

* Paraphrase, shorten, or “tidy” the prompt.
* Add examples, hints, or clarifications.
* Modify wording “just for this run”.
* Add supplier names, categories, or commercial context.
* Retry a failed run to “see if it comes out better”.
* Edit individual enriched outputs.

Any of the above constitutes **governance breach**.

---

### 3.3 Prompt Insertion Rule

For every enrichment run:

1. Open a **new chat**.
2. Paste the canonical prompt **in full**.
3. Paste the product input.
4. Execute once.

No other interaction pattern is permitted.

---

## 4. Prompt Versioning Rules

### 4.1 Prompt Version Identification

Prompt versions are identified as:

```
P0001, P0002, P0003, …
```

* Versions are **monotonic**.
* Versions are **never reused**.
* Each version corresponds to exactly one canonical prompt text.

---

### 4.2 What Constitutes a Prompt Change

A new prompt version is **required** if any of the following change:

* Inference rules
* Determinism constraints
* Tone or title policy
* Failure behaviour
* Any wording that could alter output behaviour

Cosmetic formatting changes are **not permitted** without versioning.

---

### 4.3 Relationship to Enrichment Versions

* Prompt versions control **how enrichment is performed**.
* Enrichment versions (E0001, E0002, …) record **what logic outcome is accepted**.
* A prompt change may require a new enrichment version and regeneration.

---

## 5. Failure Handling in Prompt Use

If a run fails:

1. The output is rejected.
2. The artefact is retained for audit.
3. The operator **stops immediately**.
4. The issue is escalated to the Master.
5. No re-run occurs until governance direction is given.

“Retry until it looks good” is explicitly forbidden.

---

## 6. Upstream Dependency Declaration (Mandatory)

The canonical enrichment prompt **depends on the upstream normalisation process** providing:

* A shared, supplier-agnostic logical structure.
* Explicit **REQUIRED / OPTIONAL** designation for each field.

This dependency introduces **no supplier-specific logic** into enrichment, but it **must be honoured** by upstream processes.

This requirement must be visible to ConOps and Requirements authors.

---

## 7. Completion & Handover Statement (Formal)

**To: Master – ORA Programme**

This document formally completes **Chunk 2B-2 – Prompt Discipline & Canonical Enrichment Prompt**.

The following are hereby confirmed:

1. One canonical enrichment prompt has been defined and locked.
2. Prompt discipline rules are explicit and enforceable.
3. Prompt versioning rules are explicit and auditable.
4. Failure handling is strict, deterministic, and escalation-driven.
5. Full compliance with **Chunk 2A** and **Chunk 2B-1** is confirmed.
6. No supplier-specific, category-specific, or per-product logic is introduced.

On this basis:

* **Chunk 2B-2 is COMPLETE**
* **Prompt governance is locked**
* **The Master may safely invoke Chunk 2B-3**

**End of Chunk 2B-2.**
