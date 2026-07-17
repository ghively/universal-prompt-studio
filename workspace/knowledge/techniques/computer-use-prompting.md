---
slug: computer-use-prompting
name: Computer-use prompting
lever: agentic
helps: screenshot-driven GUI agents (browser and desktop automation, computer-use tools)
hurts: API/CLI-capable tasks — pixels are the slow, expensive last resort
---
GUI agents fail by assuming actions landed. The load-bearing instruction
(Anthropic's own): "After each step, take a screenshot and carefully evaluate if
you have achieved the right outcome. Explicitly show your thinking. If not
correct, try again." Verify-after-act; screenshot-before-start to confirm the
initial state.
- **Define unexpected-state behavior**: "If a dialog, login wall, CAPTCHA, or
  grayed-out control appears, stop and describe exactly what you see — do not
  guess-click." Guess-clicking compounds; a described dead-end is recoverable.
- **Give app-specific walkthroughs** in the system prompt when the task set is
  known ("the export lives under File > Download as CSV") — cheap tokens, large
  reliability gains.
- **Prefer deterministic navigation** where equivalent: keyboard shortcuts,
  direct URLs, in-page search beat visual hunting.
- Screenshots are tokens: steer captures to verification points, not a
  continuous stream.
The screen is untrusted content — on-screen text that asks the agent to change
task gets the quarantine treatment (untrusted-content-quarantine), and
consequential actions from GUI state need gates (approval-gates).
