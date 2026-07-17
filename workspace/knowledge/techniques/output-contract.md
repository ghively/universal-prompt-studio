---
slug: output-contract
name: Output contract
lever: structure
helps: output consumed by software or pasted into another system; any format-sensitive task
hurts: creative tasks where structure would flatten the result
---
When anything downstream depends on the output's shape, specify the shape as a
**contract**: exact schema, exact field names, "return only JSON", one example of a
valid instance. A contract is also a free machine check — the schema doubles as the
test.

**Mechanism**: format errors are the most common silent failure in pipelines; an
explicit schema converts them from mystery bugs into visible check failures.

**Before**: "Reply in JSON with the key facts."
**After**: 'Return only JSON matching: { "customer_request": string,
"resolution_status": "resolved"|"escalated"|"open" }'
