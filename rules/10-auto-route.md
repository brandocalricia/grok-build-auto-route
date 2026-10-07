# Auto-route

If you are a subagent, do the assigned work yourself. Do not spawn. Depth limit is 1. On a depth-limit error, finish the work here. Do not retry spawn in a loop.

Do not switch this session's model. Do not run `/model`. Do not pass a `model` argument. `~/.grok/config.toml` pins the type. If a spawn is rejected for naming a model, retry once without it.

A parent classifies, spawns, collects with `get_command_or_subagent_output`, and answers. Those two tools are the parent's work. A parent edit is denied until this session has spawned a worker. If a spawn fails, spawn again.

No spawn for a greeting, yes/no, thanks, or a one-line fact that needs no files and no tools. Every other prompt is spawned.

One role per piece of work. Start independent pieces together. Do not spawn a second role for the same piece.

- Read or review files: `route-read` or `explore` (grok-4.5).
- Edit or test: `route-edit` (grok-4.6). One writer per path.
- Design or judgment: `route-hard` (grok-4.7), only when that piece needs it.

Leave `general-purpose` unpinned. Never select `grok-4.7-build-fast` (catalog: "Fast variant. 2x the price.").

Do not ask to confirm shell commands. Do not invent a discount. The status line shows exact routed tokens and the measured dollar gap versus grok-4.7: `4.5×N 4.6×M · saved T tok ($D)`. grok-4.6 matches grok-4.7. grok-4.5 is cheaper only on cached input.

When you ship a fix, prove it before you say it works. Spawn a tester. For a rule, hook, pin, skill, or status line, run the check in a new `grok` process. A write to disk is not proof.
