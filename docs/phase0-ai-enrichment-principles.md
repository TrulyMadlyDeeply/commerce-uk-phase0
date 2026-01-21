<!-- markdownlint-disable -->
# Phase 0 – Global AI Enrichment Principles & Governance

**Project:** Truly Madly Deeply Ltd
**Programme:** Phase 0 UK E-commerce
**Document ID:** PH0-AI-ENRICH-GOV
**Status:** AUTHORITATIVE – LOCKED
**Version:** 1.0
**Applies From:** Phase 0 onwards unless superseded by Master approval

---

## 1. Purpose and Authority

This document defines and locks the **global AI enrichment rules** for Phase 0.

It is governance-level and **takes precedence over all later Chunks** with respect to enrichment behaviour, constraints, and change control.

Once approved, **no later Chunk may reinterpret, bypass, or locally optimise enrichment behaviour** without explicit Master approval and a versioned amendment to this document.

---

## 2. Global Enrichment Philosophy

### 2.1 One Process, Always

There is **one global enrichment process** that applies to **all products**, regardless of:

* Supplier
* Category
* Product type
* Volume
* Commercial priority

There are **no per-product, per-category, or per-supplier enrichment variants**.

If an output is incorrect or undesirable, the **process is corrected**, and outputs are **regenerated**.

Manual correction of individual enriched products is **explicitly forbidden**.

---

### 2.2 Process-Correct, Not Hand-Correct

“Process-correct, not hand-correct” means:

* Humans **do not fix individual products**
* Humans **fix the rules that produced the output**
* Outputs are then **regenerated deterministically**

This rule exists to prevent:

* Silent divergence
* Untraceable edits
* Hidden quality debt
* Scale failure beyond a few thousand products

Local optimisation is rejected in favour of **systemic correctness**.

---

### 2.3 Consistency Over Local Optimisation

Consistency is prioritised because:

* Phase 0 targets **thousands to tens of thousands of products**
* Later automation depends on **predictable structure**
* Manual exceptions **do not scale**
* Auditability requires **repeatable logic**

A “good enough everywhere” outcome is superior to a “perfect in some places” outcome that cannot be reproduced.

---

## 3. Enrichment Versioning Rules

### 3.1 What Requires a New Enrichment Version

A new enrichment version **MUST** be created if **any** of the following change:

* Field derivation logic
* Normalisation rules
* Title refinement rules
* Structured output composition
* Inclusion or exclusion rules for enriched fields

Cosmetic presentation changes outside enrichment logic **do not** count.

---

### 3.2 Version Identification

Enrichment versions are identified as:

```
E0001, E0002, E0003, …
```

Version identifiers are **monotonic, never reused**, and globally unique within Phase 0.

---

### 3.3 Meaning of Regeneration

Regeneration means:

* Re-running the enrichment process for the affected product set
* Producing new structured outputs under the new version
* Leaving all prior outputs intact for audit and rollback

Regeneration **never** means hand editing generated artefacts.

---

### 3.4 What Must Never Change Across Versions

An enrichment version change **must not**:

* Require page template redesign
* Alter URL structures
* Break category or navigation logic
* Change page scaffolding assumptions
* Invalidate completed prior Chunks

If a proposed enrichment change violates any of the above, it is **out of scope** and must be escalated to the Master.

---

## 4. Enrichment Stability Guarantees

### 4.1 Structural Separation (Non-Negotiable)

The system is explicitly divided into three independent concerns:

1. **Supplier extraction logic**
2. **Enrichment logic**
3. **Page scaffolding and presentation**

Enrichment logic **must not**:

* Depend on page layout
* Assume navigation structure
* Encode template-specific behaviour

Page scaffolding **must not**:

* Encode enrichment logic
* Depend on enrichment version specifics

---

### 4.2 No Knock-On Redesign Rule

An enrichment change **must never** force:

* HTML restructuring
* Template refactors
* Category reshaping
* Sitemap logic changes
* Navigation redesign

If an enrichment change would cause knock-on redesign, the change is **invalid for Phase 0**.

---

### 4.3 Backward Safety

All enrichment versions must remain:

* Structurally compatible with earlier outputs
* Consumable by unchanged page scaffolding
* Comparable for audit and review

This ensures prior Chunks remain valid indefinitely.

---

## 5. Supplier-Agnostic Rule Boundary

### 5.1 What Enrichment Logic May Depend On

Enrichment logic may depend **only** on:

* Normalised supplier input fields
* Globally defined controlled vocabularies
* Global enrichment rules
* Explicit versioned logic

---

### 5.2 What Is Strictly Forbidden

Enrichment logic must **not** vary by:

* Supplier identity
* Product category
* Product popularity
* Commercial importance
* Manual preference

There are **no exceptions**.

---

### 5.3 Where Supplier Differences Are Permitted

Supplier-specific behaviour is permitted **only** in:

* Input mapping
* Field normalisation
* Pre-enrichment adaptation

Once input is normalised, **all products enter the same enrichment pipeline**.

---

## 6. Manual-First but Automation-Ready Constraint

Phase 0 remains manual-first in **execution**, not in **logic**.

This means:

* Humans may trigger enrichment runs
* Humans may review outputs
* Humans may approve regeneration

Humans may **not**:

* Edit individual enriched products
* Introduce silent exceptions
* Bypass versioning rules

The enrichment process must be automation-ready **without behavioural change**.

---

## 7. Governance and Change Control

* All enrichment logic is governed by this document
* Any change requires:

  * A new enrichment version
  * Regeneration of affected outputs
  * Explicit documentation
* No local, temporary, or “just this once” exceptions are allowed

This rule is absolute.

---

## 8. Completion & Handover Statement (Formal)

**To: Master – ORA Programme**

This document formally completes **Chunk 2A – Global AI Enrichment Principles & Governance**.

The following are hereby confirmed:

1. **Global enrichment principles are locked and authoritative**
2. **One enrichment process applies to all products**
3. **Manual per-product enrichment correction is forbidden**
4. **Enrichment changes require versioning and regeneration**
5. **No enrichment change may cause redesign of pages, categories, navigation, or URLs**
6. **Later Chunks must treat these rules as immutable unless explicitly amended by Master approval**

On this basis:

* **Chunk 2A is COMPLETE**
* **Governance is stable**
* **The next approved Chunk may now proceed**

**End of Chunk 2A.**
