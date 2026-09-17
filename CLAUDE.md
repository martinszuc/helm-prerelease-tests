# CLAUDE.md

Guidance for Claude (and other AI agents) working in this repo.

## fullsend agent model configuration

This repo runs [fullsend](https://fullsend.sh) in per-repo installation mode
(`.fullsend/config.yaml`, dispatched via `.github/workflows/fullsend.yaml`).
Every agent's model is pinned explicitly rather than left on fullsend's
built-in default — the built-in default alias can lag a generation behind
what's actually enabled for this project, so pinning avoids silently
running an older model.

Confirmed available models for this project (pulled from Model Garden,
not fullsend's own example docs, which referenced a different sonnet
version):

- `claude-opus-4-6`
- `claude-sonnet-4-6`

No haiku tier is enabled for this project, so no agent uses haiku.

Assignment:

| Role         | Agent name | Model               | Rationale |
|--------------|-----------|----------------------|-----------|
| coder        | `code`    | `claude-opus-4-6`    | Actual code generation/editing — worth the strongest model. |
| triage       | `triage`  | `claude-sonnet-4-6`  | Classification/routing work, not code generation. |
| review       | `review`  | `claude-sonnet-4-6`  | Review quality matters but doesn't need opus. |
| fix          | `fix`     | `claude-sonnet-4-6`  | Smaller, targeted fixes. |
| retro        | `retro`   | `claude-sonnet-4-6`  | Summarization/reflection. |
| prioritize   | `prioritize` | `claude-sonnet-4-6` | Triage-adjacent classification. |

Note the naming quirk: the `coder` **role** provisions a built-in agent
that is actually named `code` — the `agents:` entry must use `name: code`,
not `name: coder`, or the override silently won't apply.

To change a model, edit the `agents:` block in `.fullsend/config.yaml`
directly (there is no local `fullsend` CLI installed in this repo's
environment — the fullsend binary runs inside the GitHub Actions workflow,
not locally), then commit and push.
