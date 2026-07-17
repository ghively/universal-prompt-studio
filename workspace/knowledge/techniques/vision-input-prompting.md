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
- **Crop/zoom is the top technique**: giving the model a crop tool (or manually
  supplying crops of relevant regions) yields consistent gains on detail work —
  full-page screenshots at native resolution lose small text.
- **Ask for transcription before judgment** on documents/charts: "first transcribe
  the values, then analyze" — the vision analogue of quote-then-answer, catching
  misreads before they become conclusions.
- Multiple images: label them ("Image 1: before; Image 2: after") and refer by label.
- For video with text-only models: sample frames + timestamps, then treat as
  labeled multi-image input.
