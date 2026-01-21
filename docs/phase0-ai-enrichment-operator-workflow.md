<!-- markdownlint-disable -->

# Phase 0 – Manual Enrichment Execution & Operator Workflow

**Project:** Truly Madly Deeply Ltd
**Programme:** Phase 0 UK E-commerce
**Document ID:** PH0-AI-ENRICH-OPFLOW
**Status:** AUTHORITATIVE – SUBJECT TO MASTER ACCEPTANCE
**Version:** 1.0

---

## 1. Purpose and Authority

This document defines and locks the **human-operated execution workflow** for Phase 0 AI enrichment.

It specifies:

* the precise steps a human operator follows to run enrichment,
* how products are selected, prioritised, and excluded from reprocessing,
* how enrichment runs are executed using ChatGPT,
* how outputs are reviewed strictly for contract compliance,
* how artefacts are stored, named, retained, and audited,
* when execution must stop and be escalated.

This document is **operational**.
It does **not** redefine governance, prompts, schemas, or enrichment logic.

Once accepted, this workflow is **binding for Phase 0**.

---

## 2. Binding Inputs (Read-Only)

The following documents are authoritative and immutable:

1. Phase 0 – Global AI Enrichment Principles & Governance (Chunk 2A)
2. Phase 0 – AI Enrichment Run Model (Chunk 2B-1)
3. Phase 0 – Prompt Discipline & Canonical Enrichment Prompt (Chunk 2B-2)
4. Phase 0 – Canonical Enrichment Output Contract (Chunk 2B-3)

This document must be read and applied **in strict alignment** with them.

---

## 3. Operator Model (Locked)

### 3.1 Operator Identity

* There is **one operator role** for Phase 0.
* The role is singular and accountable.
* The individual name is **not embedded** in artefacts.

All audit language refers to **“the Phase 0 Operator”**.

---

## 4. Product Selection and Prioritisation

### 4.1 Inputs Used by the Operator

Each operating day, the operator receives **two explicit lists**:

1. **Changed Products List**
   Products already live whose upstream normalised inputs have changed.

2. **Never-Enriched Products List**
   Products that have never produced an accepted enrichment output.

The operator does **not** infer change status manually.

---

### 4.2 Processing Priority (Mandatory Order)

The operator MUST process in the following order:

1. Products in the **Changed Products List**
2. Products in the **Never-Enriched Products List**

Products already enriched under the **current enrichment version** and **not present** in the Changed Products List MUST NOT be reprocessed.

---

### 4.3 Reprocessing Guard Rule (Hard)

A product is considered **already enriched** if:

* An **accepted JSON output exists** for the same:

  * `product_id`
  * `enrichment_version`

If such an artefact exists, the operator MUST NOT reprocess that product unless it appears on the Changed Products List.

Manual judgement is forbidden.

---

## 5. Normalised Input Handling

### 5.1 Input Location (Authoritative)

All normalised inputs are stored in a **non-Git working folder** excluded from version control.

This folder is the **sole source** for enrichment inputs.

---

### 5.2 Input Integrity Rule

The operator:

* MUST treat normalised inputs as read-only.
* MUST NOT edit, clean, or “fix” inputs manually.
* MUST NOT add annotations or comments.

If an input is defective, enrichment must fail and be escalated.

---

## 6. Enrichment Run Execution (Exact Steps)

### 6.1 Run Setup

For **each enrichment run**, the operator MUST:

1. Open **a new ChatGPT chat**.
2. Paste the **canonical enrichment prompt verbatim**.
3. Paste **1 to 5 normalised product inputs**.
4. Execute **once only**.

No chat reuse.
No retries.
No follow-up prompts.

---

### 6.2 Run Size Constraint

* Minimum: 1 product
* Maximum: 5 products

Exceeding this range is forbidden.

---

## 7. Output Review and Validation

### 7.1 What the Operator Reviews

The operator reviews outputs **only for contract compliance**, including:

* valid JSON structure,
* required fields present,
* correct ordering and cardinality,
* compliance with hard rules (for example required fields, inference rules).

The operator MUST NOT review:

* wording preference,
* marketing quality,
* stylistic tone,
* perceived commercial effectiveness.

---

### 7.2 Acceptance Criteria

An output is **accepted** only if:

* It fully complies with the canonical output contract.
* No hard failure conditions are present.
* The output can be ingested downstream without modification.

Accepted outputs are immutable.

---

## 8. Rejection and Failure Handling

### 8.1 Rejection Handling (Mandatory)

If an output is non-compliant:

1. The output artefact is moved to a **Rejected/** folder.
2. No retry is attempted.
3. Execution **stops immediately**.

Deletion is forbidden.

---

### 8.2 Stop Rule (Absolute)

The operator MUST stop immediately if:

* the model cannot produce compliant output,
* required fields are missing,
* inference rules are violated,
* output structure deviates from contract,
* the prompt cannot be applied verbatim.

There are **no local workarounds**.

---

## 9. Escalation Rules

### 9.1 Escalation Authority

All non-compliant outcomes are escalated to:

**The Master – ORA Programme**

Escalation is immediate and unbatched.

---

### 9.2 Escalation Contents

Each escalation MUST include:

* run identifier,
* enrichment version,
* affected product IDs,
* nature of non-compliance,
* why the issue cannot be resolved locally.

The operator MUST NOT propose informal fixes.

---

## 10. Folder Structure and Artefact Management

### 10.1 Canonical Structure

```
/phase0/
  /inputs_normalised/
  /enrichment/
    /E000X/
      /accepted/
      /rejected/
  /audit/
```

---

### 10.2 Naming Rules

* Run identifiers are operator-assigned.
* Artefacts MUST include:

  * run ID
  * enrichment version
  * UTC date
* Overwrites are forbidden.

---

### 10.3 Retention Policy

All Phase 0 artefacts are retained for the **entire Phase 0 lifetime**.

Nothing is deleted or replaced.

---

## 11. Governance Boundary Reminder

The operator MUST NOT:

* modify the prompt,
* modify the output schema,
* edit generated JSON,
* retry failed runs,
* process “just this one” product differently.

All such actions are governance breaches.

---

## 12. Completion & Handover Statement (Formal)

**To: Master – ORA Programme**

This document formally completes **Chunk 2B-4 – Manual Enrichment Execution & Operator Workflow**.

The following are confirmed:

1. A single, explicit human execution workflow is defined and locked.
2. Operator behaviour is fully constrained and audit-safe.
3. Reprocessing guards prevent silent drift or duplication.
4. Failure handling is stop-first and escalation-only.
5. Artefact retention, naming, and storage rules are explicit and enforceable.
6. The workflow is compatible with later automation without behavioural change.

On this basis:

* **Chunk 2B-4 is COMPLETE**
* **Subject to Master acceptance, this Chunk may be locked**
* **No further Phase 0 enrichment execution ambiguity remains**

**End of Chunk 2B-4.**
