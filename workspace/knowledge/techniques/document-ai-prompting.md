---
slug: document-ai-prompting
name: Document-AI prompting (PDF/OCR pipelines)
lever: modality
helps: extraction from PDFs, scans, invoices, forms, financial documents
hurts: n/a — but treating documents as "just vision input" underperforms at volume
---
Document extraction is a pipeline discipline. Route by type: clean digital PDFs
→ native ingestion or specialized parsers (cheap, near-parity); handwriting and
degraded scans → frontier VLMs (the only practical option); production volume →
hybrid — OCR for bulk text, LLM for understanding and hard pages.

Rules that survive across the pipeline: extract to an explicit **schema** with a
null policy ("null when absent, never guess"); demand **structure-preserving
transcription** for tables (markdown/HTML, merged cells resolved) before any
analysis — tables are the most error-prone element; require **page-anchored
citations** ("for every field: page + verbatim source text") so misreads are
auditable; process page-batched, never one monolithic dump. On
grounding-capable models, ask for bounding boxes on extracted fields to make
verification mechanical.
