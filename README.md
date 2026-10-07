# grok-build-auto-route

**Problem:** Grok Build always uses grok-4.7 for subagents / how to route 4.5 4.6 4.7.

Without `[subagents.models]` pins, spawned workers inherit the parent (usually grok-4.7) or you pay for `grok-4.7-build-fast` ("Fast variant. 2x the price."). This drop-in routes reads to grok-4.5, edits to grok-4.6, and design/review to grok-4.7.

GitHub Actions is off on purpose. Do not add workflows.

## Install

Copy three files plus one config table. Restart Grok after any rule or pin change. Paths below use `~/.grok`. If you must use an absolute path, replace YOURHOME yourself — do not commit a hard-coded home directory.

```sh
mkdir -p ~/.grok/rules ~/.grok/agents
cp rules/10-auto-route.md ~/.grok/rules/10-auto-route.md
cp agents/route-read.md ~/.grok/agents/route-read.md
cp agents/route-edit.md ~/.grok/agents/route-edit.md
cp agents/route-hard.md ~/.grok/agents/route-hard.md
```

Append **only** the `[subagents.models]` snippet from `examples/subagents.models.toml` to `~/.grok/config.toml`. Do not replace your whole config.

```toml
[subagents.models]
explore = "grok-4.5"
route-read = "grok-4.5"
route-edit = "grok-4.6"
plan = "grok-4.7"
route-hard = "grok-4.7"
```

Also set, if you have not already:

```toml
[features]
subagent_model_inheritance = false

[subagents]
max_depth = 1
```

Leave `general-purpose` unpinned. Never select `grok-4.7-build-fast`.

## What each stub does

| Agent | Model | Use |
| --- | --- | --- |
| `route-read` (or built-in `explore`) | grok-4.5 | Search and file reading |
| `route-edit` | grok-4.6 | Edits and test runs. One writer per path |
| `route-hard` (or built-in `plan`) | grok-4.7 | Architecture, design, review |

The parent session keeps its own model. It classifies, spawns, collects with `get_command_or_subagent_output`, and answers. It does not pass a `model` argument on spawn; the pin in `config.toml` chooses the type. Depth limit is 1.

Do not spawn for a greeting, yes/no, thanks, or a one-line fact that needs no files and no tools.

## Check

```sh
grok inspect --json
```

Confirm `subagents.models` shows the pins above and `features.subagent_model_inheritance` is false. A new session must be started after the copy.

## Turn it off

Delete `~/.grok/rules/10-auto-route.md` and the `[subagents.models]` table, then restart Grok.

## License

MIT
