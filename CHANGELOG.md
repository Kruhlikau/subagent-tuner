# Changelog

## 0.1.7
- Auto no longer leaves a built-in agent without a recommendation when Claude Code announces it after the call: the call is judged by the type's name (anything unknown is custom), and at the agent's spawn, which says who provides the type, the recommended model is applied if the engine provides it. The spawn takes a model only, so the effort recommendation is not applied in that case. Manual and a saved default are unchanged. Checked in live sessions of Claude Code 2.1.295 and 2.1.300: the announcement can come after the call and the spawn.

## 0.1.6
- The stated minimum is Claude Code 2.1.295, the oldest version the mod was tested on (0.1.5 relies on the `agent.offer` event, whose first version was not confirmed). `plugin.json` has no field for a minimum, so it is in the README and the description.

## 0.1.5
- A custom agent's own model and effort are no longer overridden when no default is saved. The mod cannot read an agent's definition, so it now notes which agent types the engine offers as built-in; for any other type the recommendation is `inherit`: Auto leaves the call untouched and Manual's pane starts on `inherit`. A saved default still applies, and a field left at `rec.` inherits. Built-in agents are recommended as before (noted by the k-mods maintainer).
- README: Manual is the mode after install and what happens when a question goes unanswered.

## 0.1.4
- The pane no longer starts the subagent after a Cancel when the answer file could not be written: a decision recorded before the wait ran out is honoured (Cancel cancels, Run and save still saves).
- A plugin folder that takes no write now falls back to the dialogs at once, instead of a pane the person would wait ten minutes on.
- The pane's buttons share a row from the width their labels need, so the long Run label (inherited model and effort) no longer runs past the edge.

## 0.1.3
- Auto: the saved default now wins over the agent. It replaces a model or effort the call set itself; a field left at `rec.` keeps the call's value, else the recommendation.

## 0.1.2
- The pane works on Windows: the wait runs in `sh` or `powershell.exe`, and the sentinel is written through `$.fs` (no `mktemp`, `touch` or `rm` needed).

## 0.1.1
- `claude -p` runs and SDK hosts are left alone: subagents start exactly as the agent asked, so eval runs are not skewed.

## 0.1.0
- First release: recommendation by task kind, Manual and Auto modes, the band above the prompt, the pick pane and the dialog chain.
