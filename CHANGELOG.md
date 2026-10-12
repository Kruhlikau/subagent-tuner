# Changelog

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
