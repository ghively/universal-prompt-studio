---
slug: confidence-elicitation
name: Confidence elicitation
lever: verification
helps: factual/analytical tasks consumed downstream; countering both over-hedging and false confidence
hurts: creative tasks; adds a field consumers must handle
---
Make uncertainty explicit and structured instead of tonal: add a confidence field
to the output contract ("confidence: high|medium|low, with the reason") and an
abstention path ("if you cannot verify, say CANNOT_VERIFY rather than guessing").

Two failure modes this fixes: models hedging in prose ("it might possibly...")
which poisons downstream parsing, and models stating guesses fluently. Verbal
labels are better calibrated than numeric ones ("give a probability" invites
false precision — prefer high/medium/low with named reasons). Self-reported
confidence is signal, not truth — treat it like the interpretation report.
