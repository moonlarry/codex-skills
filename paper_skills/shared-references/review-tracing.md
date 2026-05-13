# Review Tracing

Write review traces under `paper/review-traces/<skill>/<date>_run<NN>/`.

## Minimum Files

- `prompt.md`: the exact bounded reviewer task and input list.
- `response.md`: the raw reviewer findings or verdict.
- `handoff.md`: executor summary of actions taken, skipped items, validation, and remaining risk.
- `inputs.json`: paths, hashes, and timestamps for reviewed artifacts.

## Trace Levels

- `full`: write all minimum files.
- `summary`: write `handoff.md` and `inputs.json`.
- `off`: only skip when the parent workflow explicitly allows it.

## Rules

- Keep traces project-local.
- Do not store secrets or API keys.
- Prefer relative paths for files inside the paper directory and absolute paths for external result files.
