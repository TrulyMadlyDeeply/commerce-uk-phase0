<!-- markdownlint-disable -->

# Master Handover Pack – Phase 0 UK E-commerce Website (Cloudflare Pages)

## 1. Purpose of This Handover

This file is the single authoritative handover pack for continuing the Phase 0 UK e-commerce website build on Cloudflare Pages using a chunk-based delivery process.

It exists to:
- prevent drift and greenfield rewrites,
- preserve all locked decisions,
- ensure future Chunks work incrementally from the current repo baseline,
- keep manual AI enrichment as the intentional Phase 0 operating model.

## 2. Current Baseline (Authoritative)

- Live site is deployed on Cloudflare Pages with Git-linked auto-deploy.
- Repo is the source of truth.
- Phase 0 design target is up to roughly 20,000 products, UK-only, GBP, English.

Critical principle:
- Static-first, not static-only.
- Duplication must be extremely low.

## 3. Locked Enrichment Governance Stack (Authoritative)

These documents are locked and must be treated as immutable unless explicitly amended by Master approval:

1. /docs/phase0-ai-enrichment-principles.md
   - Global rules: one enrichment process, no per-product edits, versioning and regeneration only.

2. /docs/phase0-ai-enrichment-run-model.md
   - Run model for how enrichment is executed.

3. /docs/phase0-ai-enrichment-prompt.md
   - Prompt discipline rules and canonical prompt P0001.
   - Outputs must be as close to text-identical as the model allows.
   - Hard failure behaviour, no partial outputs.

4. /docs/phase0-ai-enrichment-output-contract.md
   - Canonical output contract OC0001 v1.0.
   - JSON root envelope with run metadata and 1–5 products per run.
   - ValueWithProvenance with SUPPLIED, DERIVED, INFERRED.

5. /docs/phase0-ai-enrichment-operator-workflow.md
   - Manual operator workflow, stop and escalate rules, accepted and rejected artefact handling.

6. /docs/phase0-ai-enrichment-integration-boundary.md
   - Integration boundary contract.
   - Downstream consumers must treat accepted enrichment outputs as immutable inputs.
   - Downstream must not reinterpret, infer, enrich, or override.

Net effect:
- Enrichment is sealed.
- Downstream Chunks must consume enrichment outputs without changing them.

## 4. Locked Delivery Control Rules

- All work is delivered in Chunks.
- Each Chunk produces one file-ready artefact or a bounded set of repo edits.
- Each Chunk must end with a clear completion and handover statement.
- If a Chunk encounters an issue requiring Master approval, it must stop and draft an escalation note, it must not proceed.

## 5. Operating Environment

- Windows 11.
- VS Code with Git integration for edits, commits, and pushes.
- Instructions must be explicit, step-by-step, and repo-first.

## 6. Phase 0 Manual AI Enrichment Reality

Manual execution is intentional and first-class:
- Operator selects products to process.
- Operator runs ChatGPT enrichment using the locked prompt and contract.
- Outputs are committed as governed artefacts.
- If outputs are wrong, the process is corrected and regenerated, manual per-product corrections are forbidden.

Images:
- Missing images may be supplemented with AI-generated lifestyle images, target at least 2000 x 2000.
- Detailed image-generation workflow is downstream of the enrichment governance stack and must not break it.

## 7. Immediate Next Stage After This Handover

Move from enrichment governance into website consumption Chunks:
- category and sub-category navigation,
- a small representative set of real pages (category and product),
- consumption pipeline that reads accepted enrichment JSON and generates static pages without altering enrichment content,
- tracking hooks for PPC and analytics are mandatory.

## 8. Known Governance Correction (Historic)

A previous Chunk introduced a Cloudflare Pages build pipeline without prior Master approval. This was treated as a governance correction and required explicit acknowledgement. Future Chunks must not introduce baseline changes without Master escalation and approval.

End of handover.
