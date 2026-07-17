---
slug: vision-input-prompting
name: Vision-input prompting
lever: modality
helps: tasks where the model reads images (screenshots, documents, photos, charts)
hurts: n/a
---
- **Images before the question**: like documents, media goes first, the ask last.
- **Direct attention explicitly**: "look at the totals row of the table" outperforms
  hoping the model finds it; models read images coarsely unless pointed.
- **Crop/zoom**: supplying a crop tool (or manual crops) yields consistent gains
  on detail work — with a platform caveat: Gemini 3's Agentic Vision crops/zooms/
  annotates natively, making manual crop-supply the fallback, not the universal
  top move. New promptable surface on grounding-capable models: coordinate
  requests ("return [ymin,xmin,ymax,xmax] for each ...") for mechanical
  verification.
- **Ask for transcription before judgment** on documents/charts: "first transcribe
  the values, then analyze" — the vision analogue of quote-then-answer, catching
  misreads before they become conclusions.
- Multiple images: label them ("Image 1: before; Image 2: after") and refer by label.
- For video with text-only models: sample frames + timestamps, then treat as
  labeled multi-image input.

**Tables are the hardest element** (merged cells, multi-column spans, dense
financials): transcribe to markdown/HTML with spans resolved before any
analysis, and verify cell-level. Document pipelines at volume: see
document-ai-prompting.
