---
slug: claim-verification-pipeline
name: Claim decomposition + verification
lever: grounding
helps: long-form factual generation (reports, bios, briefs); post-hoc hallucination audits of any prose
hurts: cost/latency-sensitive paths (N verification calls per output); short answers; subjective or creative text
---
Chain-of-Verification, as a chain or pipeline: (1) draft; (2) extract atomic,
decontextualized claims — each must be checkable standing alone; (3) verify each
claim independently against provided sources or retrieval, phrasing verification as
fresh questions answered WITHOUT sight of the draft; (4) rewrite from verified
claims only, flagging the rest. This is the FActScore → SAFE → VeriScore lineage
turned from an eval into a production control; VeriScore's refinement — extract only
*verifiable* claims, skip opinions and hedges — is the version that survives contact
with real text.

Two caveats from the 2024-26 literature: over-decomposition backfires ("Decomposition
Dilemmas": too-atomic fragments lose context and burden the verifier — split to the
smallest unit that is still checkable in isolation), and verifier independence is the
active ingredient — a verifier shown the draft's reasoning rubber-stamps it.

**Mechanism**: fluent long-form output hides individual false atoms behind overall
plausibility. Decomposition converts one ungradable essay into N binary checks, and
independent re-derivation breaks the self-consistency bias that makes a model
approve its own errors.
