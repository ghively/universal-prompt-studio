---
slug: human-in-the-loop-gates
name: Human-in-the-loop gates
lever: verification
helps: irreversible or high-blast-radius actions; judge calibration; cold-start evals
hurts: throughput; over-firing gates train humans to rubber-stamp (approval fatigue)
---
Humans are the scarcest verifier — spend them where automation is blind:
- **Calibrating judges**: sample verdicts against human grades; ~85-90%
  agreement is the working bar before a judge runs unsupervised [practitioner
  consensus, not peer-reviewed].
- **Reviewing escalations**: the low-confidence bucket from uncertainty
  routing, not the full stream.
- **Approving irreversible actions**: deletions, sends, payments — gate the
  action, not the whole task (see approval-gates for the agent-side design).
Two rules: every human verdict becomes a golden-set item, so review hours
compound instead of evaporating; and monitor approval rate — a gate approving
~100% is either unnecessary or rubber-stamped, and both mean it verifies
nothing.
