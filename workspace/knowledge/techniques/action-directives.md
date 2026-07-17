---
slug: action-directives
name: Action directives
lever: clarity
helps: agentic prompts and tool-using assistants; any prompt where "suggest" vs "do" matters
hurts: nothing
---
Current models follow instructions literally: "can you suggest some changes?" gets
suggestions, not changes. Say which you want: "Change this function to..." acts;
"Recommend changes to..." advises.

To set a standing default, add one block to the system prompt — either
`<default_to_action>` (implement rather than suggest; infer the most useful action
and proceed) or `<do_not_act_before_instructions>` (research and recommend; only
edit when explicitly asked). Pick per use case; don't leave it implicit.
