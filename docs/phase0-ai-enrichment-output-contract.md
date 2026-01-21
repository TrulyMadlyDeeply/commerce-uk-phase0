<!-- markdownlint-disable -->
# Phase 0 – Canonical Enrichment Output Contract (Structured Data Contract)

**Project:** Truly Madly Deeply Ltd
**Programme:** Phase 0 UK E-commerce
**Document ID:** PH0-AI-ENRICH-OUTCONTRACT
**Status:** AUTHORITATIVE – LOCKED (on Master acceptance)
**Contract ID:** OC0001
**Contract Version:** 1.0
**Applies From:** Phase 0 onwards unless superseded by Master approval
**Depends On (binding, read-only):**

* Phase 0 – Global AI Enrichment Principles & Governance (Chunk 2A) 
* Phase 0 – AI Enrichment Run Model (Chunk 2B-1) 
* Phase 0 – AI Enrichment Prompt Discipline (Chunk 2B-2) 

---

## 1. Purpose and Authority

This document defines the **single canonical structured output contract** produced by the Phase 0 enrichment process.

This contract is **supplier-agnostic** and **category-agnostic**. It applies to **all products** and supports **1 to 5 products per run**.

This contract is designed to:

1. Support strict determinism intent (outputs as close to text-identical as possible).
2. Support hard failure behaviour (no partial outputs).
3. Prevent per-product, per-category, or per-supplier structural variants.
4. Remain stable across enrichment versions without forcing redesign of downstream page scaffolding, navigation, URLs, or sitemap logic. 

---

## 2. Canonical Output Contract

### 2.1 Output Format Requirements

1. Output MUST be **pure structured data**.
2. Output MUST be **valid JSON**, encoded as UTF-8.
3. Output MUST contain **exactly one JSON object** at the root.
4. Output MUST NOT contain:

   * markdown fences
   * prose commentary
   * explanations
   * “notes to the user”
5. All keys and enum values in this contract are **case-sensitive**.

### 2.2 Root Object Structure

#### 2.2.1 Root Object (Required)

| Field                | Required | Type   | Constraints                                                            |
| -------------------- | -------: | ------ | ---------------------------------------------------------------------- |
| `contract_id`        |      Yes | string | Must equal `"OC0001"`                                                  |
| `contract_version`   |      Yes | string | Must equal `"1.0"`                                                     |
| `enrichment_version` |      Yes | string | Format `E` + 4 digits, example `"E0001"`                               |
| `prompt_id`          |      Yes | string | Must equal the canonical prompt ID (as governed in prompt discipline)  |
| `run`                |      Yes | object | See 2.2.2                                                              |
| `products`           |      Yes | array  | 1 to 5 items, each item is a Product Object (2.3)                      |

#### 2.2.2 `run` Object (Required)

| Field          | Required | Type    | Constraints                                                                                   |
| -------------- | -------: | ------- | --------------------------------------------------------------------------------------------- |
| `run_id`       |      Yes | string  | Operator-supplied identifier, stable for traceability. 1–64 chars. Allowed: `A–Z a–z 0–9 _ -` |
| `run_date_utc` |      Yes | string  | ISO 8601 date, format `YYYY-MM-DD`                                                            |
| `market`       |      Yes | string  | Must equal `"UK"` in Phase 0                                                                  |
| `currency`     |      Yes | string  | Must equal `"GBP"` in Phase 0                                                                 |
| `vat_included` |      Yes | boolean | Must be `true` for consumer pricing display in Phase 0                                        |

### 2.3 Product Object

Each product in `products[]` MUST follow this exact structure.

#### 2.3.1 Product Object Fields (Required vs Optional)

**A. Identity and Traceability**

| Field            | Required | Type   | Constraints                                                                                       |
| ---------------- | -------: | ------ | ------------------------------------------------------------------------------------------------- |
| `product_id`     |      Yes | string | Supplier-agnostic internal identifier for Phase 0 outputs. 1–80 chars. Allowed: `A–Z a–z 0–9 _ -` |
| `source`         |      Yes | object | See 2.3.2                                                                                         |
| `content`        |      Yes | object | See 2.3.3                                                                                         |
| `commerce`       |      Yes | object | See 2.3.4                                                                                         |
| `media`          |      Yes | object | See 2.3.5                                                                                         |
| `classification` |      Yes | object | See 2.3.6                                                                                         |
| `availability`   |      Yes | object | See 2.3.7                                                                                         |

**B. Optional blocks (present with explicit nulls only where permitted)**

| Field             | Required | Type           | Constraints                                 |
| ----------------- | -------: | -------------- | ------------------------------------------- |
| `personalisation` |       No | object or null | If present and non-null, must follow 2.3.8  |
| `compliance`      |       No | object or null | If present and non-null, must follow 2.3.9  |
| `seo`             |       No | object or null | If present and non-null, must follow 2.3.10 |

### 2.3.2 `source` Object (Required)

Purpose: ensure every enriched product is traceable back to the upstream supplier record without changing schema by supplier.

| Field                  | Required | Type   | Constraints                                                                     |
| ---------------------- | -------: | ------ | ------------------------------------------------------------------------------- |
| `supplier_key`         |      Yes | string | 1–40 chars. Example `"PMC"`                                                     |
| `supplier_product_ref` |      Yes | string | Supplier’s own identifier (SKU or equivalent). 1–80 chars                       |
| `input_record_hash`    |      Yes | string | Deterministic hash identifier for the normalised input record, 16–128 hex chars |
| `input_designation`    |      Yes | object | See 2.3.2.1                                                                     |

#### 2.3.2.1 `input_designation` Object (Required)

Purpose: carry forward the **upstream required vs optional field designations** (as produced by the normalisation process), without embedding supplier logic here.

| Field                     | Required | Type    | Constraints                                                                             |
| ------------------------- | -------: | ------- | --------------------------------------------------------------------------------------- |
| `required_fields_present` |      Yes | boolean | Must be `true` if the output is compliant, otherwise the run must hard-fail (no output) |
| `missing_required_fields` |      Yes | array   | Must be empty array `[]` for any compliant output                                       |

**Rule:** In a compliant output, `required_fields_present` MUST be `true` and `missing_required_fields` MUST be `[]`. If not, the enrichment run MUST hard-fail and produce no output.

### 2.3.3 `content` Object (Required)

| Field               | Required | Type           | Constraints                                                          |
| ------------------- | -------: | -------------- | -------------------------------------------------------------------- |
| `title`             |      Yes | object         | ValueWithProvenance (2.4) with `value` 1–180 chars                   |
| `brand`             |      Yes | object         | ValueWithProvenance with `value` 1–60 chars                          |
| `short_description` |      Yes | object         | ValueWithProvenance with `value` 1–300 chars                         |
| `long_description`  |      Yes | object         | ValueWithProvenance with `value` 1–4000 chars                        |
| `bullet_points`     |      Yes | array          | 3–8 items, each item is ValueWithProvenance with `value` 1–250 chars |
| `materials`         |       No | array          | 0–10 items, each item ValueWithProvenance, `value` 1–40 chars        |
| `dimensions_mm`     |       No | object or null | See 2.3.3.1                                                          |
| `weight_g`          |       No | object or null | ValueWithProvenance with numeric `value` (integer) 1–500000          |
| `colour`            |       No | object or null | ValueWithProvenance, `value` 1–40 chars                              |
| `occasion`          |       No | array          | 0–10 items, each item ValueWithProvenance, `value` 1–40 chars        |

#### 2.3.3.1 `dimensions_mm` (Optional)

If present and non-null:

| Field    | Required | Type   | Constraints                           |
| -------- | -------: | ------ | ------------------------------------- |
| `length` |      Yes | object | ValueWithProvenance, integer 1–200000 |
| `width`  |      Yes | object | ValueWithProvenance, integer 1–200000 |
| `height` |      Yes | object | ValueWithProvenance, integer 1–200000 |

### 2.3.4 `commerce` Object (Required)

| Field               | Required | Type           | Constraints                                                        |
| ------------------- | -------: | -------------- | ------------------------------------------------------------------ |
| `price_gbp_inc_vat` |      Yes | object         | ValueWithProvenance, numeric `value` with 2 dp, range 0.01–9999.99 |
| `rrp_gbp_inc_vat`   |       No | object or null | ValueWithProvenance, numeric 0.01–9999.99                          |
| `vat_rate`          |      Yes | object         | ValueWithProvenance, numeric `value` allowed: `0`, `5`, `20`       |
| `shipping_policy`   |      Yes | object         | See 2.3.4.1                                                        |

#### 2.3.4.1 `shipping_policy` Object (Required)

| Field               | Required | Type   | Constraints                       |
| ------------------- | -------: | ------ | --------------------------------- |
| `dispatch_days_max` |      Yes | object | ValueWithProvenance, integer 0–60 |
| `delivery_days_max` |      Yes | object | ValueWithProvenance, integer 0–60 |
| `delivery_region`   |      Yes | string | Must equal `"UK"` in Phase 0      |

### 2.3.5 `media` Object (Required)

| Field                        | Required | Type    | Constraints                                                               |
| ---------------------------- | -------: | ------- | ------------------------------------------------------------------------- |
| `primary_image_url`          |      Yes | object  | ValueWithProvenance, `value` must be absolute URL, 1–500 chars            |
| `image_urls`                 |      Yes | array   | 1–20 items, each item ValueWithProvenance URL                             |
| `image_minimum_standard_met` |      Yes | boolean | Must be `true` for compliant output                                       |
| `synthetic_images_present`   |      Yes | boolean | `true` if any image is AI-generated (Phase 0 may be false-only initially) |

### 2.3.6 `classification` Object (Required)

Purpose: enable stable site navigation and sitemap generation without forcing redesign across versions. 

| Field           | Required | Type   | Constraints                                                   |
| --------------- | -------: | ------ | ------------------------------------------------------------- |
| `product_type`  |      Yes | object | ValueWithProvenance, `value` 1–60 chars                       |
| `category_path` |      Yes | array  | 1–5 items, each item ValueWithProvenance, `value` 1–60 chars  |
| `tags`          |       No | array  | 0–30 items, each item ValueWithProvenance, `value` 1–40 chars |

### 2.3.7 `availability` Object (Required)

| Field                  | Required | Type           | Constraints                                            |
| ---------------------- | -------: | -------------- | ------------------------------------------------------ |
| `status`               |      Yes | string         | Enum: `"in_stock"`, `"out_of_stock"`, `"discontinued"` |
| `stock_quantity`       |      Yes | object         | ValueWithProvenance, integer 0–1000000                 |
| `restock_rule_applied` |       No | object or null | ValueWithProvenance, `value` 1–120 chars               |

### 2.3.8 `personalisation` Object (Optional)

If present and non-null:

| Field               | Required | Type    | Constraints                                          |
| ------------------- | -------: | ------- | ---------------------------------------------------- |
| `is_personalisable` |      Yes | boolean |                                                      |
| `fields`            |      Yes | array   | 0–20 items, each item PersonalisationField (2.3.8.1) |
| `validation`        |      Yes | object  | See 2.3.8.2                                          |

#### 2.3.8.1 PersonalisationField

| Field             | Required | Type           | Constraints                                                              |
| ----------------- | -------: | -------------- | ------------------------------------------------------------------------ |
| `field_key`       |      Yes | string         | 1–40 chars. Allowed: `A–Z a–z 0–9 _ -`                                   |
| `label`           |      Yes | object         | ValueWithProvenance, `value` 1–60 chars                                  |
| `type`            |      Yes | string         | Enum: `"short_text"`, `"long_text"`, `"numeric"`, `"date"`, `"dropdown"` |
| `max_length`      |       No | object or null | ValueWithProvenance, integer 1–500                                       |
| `required`        |      Yes | boolean        |                                                                          |
| `allowed_charset` |      Yes | string         | Enum: `"ascii"` (Phase 0)                                                |
| `example`         |       No | object or null | ValueWithProvenance, `value` 0–120 chars                                 |
| `options`         |       No | array          | For dropdown only. 0–200 items, each item ValueWithProvenance 1–60 chars |

#### 2.3.8.2 `personalisation.validation`

| Field                       | Required | Type    | Constraints    |
| --------------------------- | -------: | ------- | -------------- |
| `profanity_filter_required` |      Yes | boolean | Must be `true` |
| `legal_rejects_required`    |      Yes | boolean | Must be `true` |

(These requirements are strategic constraints for brand protection and UK law alignment. )

### 2.3.9 `compliance` Object (Optional)

If present and non-null:

| Field         | Required | Type           | Constraints                              |
| ------------- | -------: | -------------- | ---------------------------------------- |
| `pii_present` |      Yes | boolean        | Must be `false` in Phase 0 outputs       |
| `gdpr_notes`  |       No | object or null | ValueWithProvenance, `value` 0–300 chars |

### 2.3.10 `seo` Object (Optional)

If present and non-null:

| Field                | Required | Type           | Constraints                                                                                 |
| -------------------- | -------: | -------------- | ------------------------------------------------------------------------------------------- |
| `slug`               |      Yes | object         | ValueWithProvenance, `value` 1–120 chars, lowercase `a–z 0–9 -`, no spaces, no trailing `-` |
| `canonical_url_path` |      Yes | object         | ValueWithProvenance, `value` must start with `/`, 1–160 chars                               |
| `meta_title`         |       No | object or null | ValueWithProvenance, `value` 1–180 chars                                                    |
| `meta_description`   |       No | object or null | ValueWithProvenance, `value` 1–300 chars                                                    |

**Rule:** If `seo` is present, both `slug` and `canonical_url_path` MUST be present and non-null.

---

## 3. Inference Representation Rules

### 3.1 ValueWithProvenance (Canonical Type)

Many fields in this contract are not raw primitives. They are wrapped to enforce consistent inference marking without adding commentary.

**ValueWithProvenance object:**

| Field        | Required | Type             | Constraints                                   |
| ------------ | -------: | ---------------- | --------------------------------------------- |
| `value`      |      Yes | string or number | As constrained by the field using it          |
| `provenance` |      Yes | string           | Enum: `"SUPPLIED"`, `"DERIVED"`, `"INFERRED"` |

**Definitions:**

* `"SUPPLIED"` means present in the normalised supplier input (possibly cleaned/trimmed).
* `"DERIVED"` means deterministically computed from supplied fields using fixed rules (for example, slug from title).
* `"INFERRED"` means not present upstream, estimated by the enrichment process.

### 3.2 Hard Rules for Inference

1. If a value is not explicitly present upstream and not strictly derivable, it MUST be marked `"INFERRED"`.
2. The contract permits inference only if the output still satisfies the hard failure rules (required fields are present and valid).
3. Inference MUST NOT introduce variable-style “confidence scores”, probabilistic language, or free-text explanations.

### 3.3 Missing Data Representation

To preserve determinism and contract stability:

1. **Required fields** MUST always be present and MUST NOT be `null`.
2. **Optional fields**:

   * MAY be entirely absent, or
   * MAY be present with value `null`, but only where explicitly allowed above.
3. Empty string is forbidden as a “missing value marker”. If a string value is unknown and the field is optional, use `null` (if allowed) or omit the field.
4. Arrays:

   * Required arrays MUST meet minimum item counts.
   * Optional arrays MAY be present as `[]` when there are no items, if the field is included.

---

## 4. Multi-product Run Structure

### 4.1 Product Count

`products` MUST contain between **1 and 5** product objects.

### 4.2 Ordering Rules (Determinism)

1. `products[]` MUST be sorted ascending by `product_id`.
2. `image_urls[]` MUST be sorted lexicographically by the contained `value` string.
3. `category_path[]` MUST represent top-to-bottom hierarchy and MUST NOT reorder between runs for the same product and enrichment version.
4. JSON object keys MUST be emitted in the exact order defined in this contract section sequence:

   * Root keys: `contract_id`, `contract_version`, `enrichment_version`, `prompt_id`, `run`, `products`
   * Product keys: `product_id`, `source`, `content`, `commerce`, `media`, `classification`, `availability`, then optional blocks in order: `personalisation`, `compliance`, `seo`

(Where the underlying JSON writer cannot guarantee key order, the enrichment prompt MUST enforce it by explicit key ordering in the output.)

### 4.3 Stable Product Identifiers

* `product_id` is the **Phase 0 internal stable identifier** used for:

  * static page filename or route mapping
  * sitemap inclusion
  * downstream ingestion
* `source.supplier_key` and `source.supplier_product_ref` preserve traceability back to the upstream record.

This design is supplier-agnostic because:

* the structure does not change by supplier,
* supplier identity is data, not schema.

---

## 5. Stability and Versioning Notes

### 5.1 Relationship to Enrichment Versioning

* Enrichment versions (`E0001`, `E0002`, …) govern **logic changes** and regeneration requirements. 
* This output contract is designed so enrichment version changes MUST NOT require changes to downstream page scaffolding, navigation, URLs, or sitemap logic. 

### 5.2 Contract Versioning

* Contract versions are `1.0`, `1.1`, etc.
* Any contract change MUST be versioned.
* Contract version changes MUST be monotonic and never reused for different meaning.

### 5.3 What Constitutes a Breaking Change

A breaking change is any change that would force downstream consumers to change code or templates, including:

1. Removing a required field.
2. Renaming any field.
3. Changing the type of any field.
4. Changing required cardinality (for example, bullets from 3–8 to 1–8).
5. Changing the meaning of enums.

Breaking changes are disallowed in Phase 0 unless Master-approved and accompanied by explicit downstream migration instructions.

### 5.4 Non-breaking Changes

Non-breaking changes include:

1. Adding a new optional field or optional block.
2. Adding a new enum value only if downstream is documented to treat unknown enums safely (default to hard-fail or safe-ignore), and this is explicitly planned.

In Phase 0, preference is to **avoid** even non-breaking drift unless it is clearly necessary and governed.

---

## 6. Completion & Handover Statement (Formal)

**To: Master – ORA Programme**

This document formally completes **Chunk 2B-3 – Canonical Enrichment Output Structure (Structured Data Contract)**.

The following are hereby confirmed:

1. **One canonical output contract (OC0001 v1.0) is defined and locked on acceptance.**
2. **The contract is compatible with Chunk 2A governance, Chunk 2B-1 run model, and Chunk 2B-2 prompt discipline**, including the requirement to output structured data only and to hard-fail if compliance cannot be achieved.
3. **The contract is supplier-agnostic and category-agnostic** and supports **1–5 products per run** with deterministic ordering rules.
4. **Missing data and inference are handled explicitly** using ValueWithProvenance with mandatory `"INFERRED"` marking where applicable, without commentary.
5. **The contract is stable for downstream Chunks without redesign**, and enrichment version changes can occur without breaking page scaffolding, navigation, URLs, or sitemap logic.

On this basis:

* **Chunk 2B-3 is COMPLETE**
* **The Master may safely invoke the next approved sub-chunk**

**End of Chunk 2B-3.**
