# subagent-tuner

**A Claude Code mod that picks the model and reasoning effort of every subagent, so a review runs on Opus and a file search doesn't.**

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-0.1.3-informational.svg)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-2.1.287%2B-d97757.svg)](https://claude.com/blog/claude-code-mods)

<img src="docs/media/band.png" alt="The tuner's band above the prompt: Tuner, Manual, Auto (active), default sonnet / high, Reset" width="696">

When the agent starts a subagent, the tuner settles its model and effort: by asking you, by a
recommendation for the kind of task, or by your saved default.

## Install

Inside Claude Code (2.1.287 or later):

```
/plugin install subagent-tuner --marketplace Kruhlikau/subagent-tuner
```

Answer `y` to add the marketplace and pick the **user** scope. It starts working in that session.

<details>
<summary>Other ways: two steps, the shell, or a local clone</summary>

```
/plugin marketplace add Kruhlikau/subagent-tuner
/plugin install subagent-tuner@subagent-tuner
```

```sh
claude plugin install subagent-tuner --marketplace Kruhlikau/subagent-tuner
```

```sh
git clone https://github.com/Kruhlikau/subagent-tuner
claude --plugin-dir ./subagent-tuner
```

Update with `/plugin marketplace update subagent-tuner`, then `/plugin update subagent-tuner@subagent-tuner`.
Remove with `/plugin uninstall subagent-tuner@subagent-tuner`.
</details>

## What you see

**The band above the prompt** holds the mode and your saved default. Click `Manual` or `Auto` to
switch, click the model or the effort to cycle it (`rec.` means "use the recommendation"),
`Reset` to clear the default. Both are kept across sessions.

**Manual** (the default after install) asks before a subagent starts. On a terminal 144 columns
wide or more the question is a pane beside the transcript:

```
general-purpose · explore wide
Model: haiku ▾   Effort: low ▾
r: Run   d: Run and save as default   c: Cancel
```

On a narrower terminal it is a short chain of dialogs. A dismissed question starts the subagent
with what was shown; nothing is ever held back.

**Auto** never asks. Your saved default wins, even over a model or effort the agent set itself
(say, a rule in `CLAUDE.md` that makes it pass `model: "opus"`). A field left at `rec.` keeps
what the call set, else takes the recommendation. A toast says what was applied:
`Subagent (overridden) -> sonnet/high`.

## The recommendation

| Task kind (from the subagent's type, else its description) | Model | Effort |
| --- | --- | --- |
| review, audit, verify, adversarial, security | `opus` | `high` |
| implement, build, fix | `sonnet` | `medium` |
| explore, search, research | `haiku` | `low` |

A prompt longer than 4,000 characters gets one step more effort. Any other task gets no
recommendation and inherits the session's model and effort.

## Where it stays out of the way

- **`claude -p` and SDK hosts:** nothing is changed, so scripted runs and evals measure what they ask for.
- **Forks:** never touched; a fork ignores model and effort anyway.
- **VS Code extension:** it can't show the band there, so it never asks and behaves as Auto.
- **Manual:** a call that already set both its model and its effort goes through as set.

## What it can read, write and run

- **Reads** each Agent call's subagent type, description, model and effort, and the length of its prompt. It never sends anything anywhere: no network calls.
- **Changes** only the `model` and `effort` fields of Agent calls.
- **Stores** the mode and the saved default in Claude Code's own plugin store.
- **Runs** `sh` (macOS, Linux) or `powershell.exe` (Windows), only to wait for your answer in the pane: once to check which shell exists, then one waiting process per open pane.
- **Writes** an empty sentinel file to `.waits/` in the plugin's own folder when you answer in the pane; the waiting process deletes it.

## Requirements

Claude Code 2.1.287 or later (mods). Tested on 2.1.295 and 2.1.296 on macOS and on Windows 11
with Git for Windows' default settings.

## Troubleshooting

- **Dialogs instead of the pane:** the terminal is narrower than 144 columns. Widen it or zoom out.
- **Subagents still on Opus:** you are in Manual and the agent set the model itself, or Auto has no saved default for the model. Switch to Auto and set the default.
- **An update didn't change anything:** run the update commands above. Claude Code runs the installed copy from its cache, keyed by version.
- **Two bands or two toasts:** the mod is loaded twice, for example installed and also passed with `--plugin-dir`. Keep one.

## Develop

```sh
git clone https://github.com/Kruhlikau/subagent-tuner && cd subagent-tuner
claude plugin validate --strict .
claude plugin test .
claude --plugin-dir .
```

`hooks/register.tsx` holds every call on `$`; the other modules are pure. The tests run against a
fake `$` in `hooks/harness.ts`. Once Claude Code has loaded the mod, `.claude-plugin/types/` holds
the API types and `npx tsc -p .` type-checks it. Bump `version` in `.claude-plugin/plugin.json` with
every change: installed copies are cached by version.

## License

[MIT](LICENSE)
