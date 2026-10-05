# coach

A standing decision-guardian skill for AI agents. It flags traps in plans, choices, and reasoning
**while there is still time to reverse them** — and it keeps score of whether its own flags were
right.

Most "advice" from an AI is either absent or noise. This skill is the middle: it watches for the
failure patterns that actually cost people months (sunk cost, scope creep, tooling accumulation,
spending before validation, arriving late to a trend, ignoring measured results) and says something
only when the cost is material.

## Install

**Any skills-compatible agent** (Claude Code, Codex CLI, OpenCode, Copilot, Cursor, Gemini CLI, …):

```bash
npx skills add pantha704/coach-skill
```

**Manual** — copy this folder into your agent's skills directory:

| Agent | Path |
|---|---|
| OpenCode | `~/.config/opencode/skills/coach/` |
| Claude Code | `~/.claude/skills/coach/` |
| Shared hub (recommended) | `~/.agents/skills/coach/` |

Then create the state directory (first use can also do it automatically):

```bash
mkdir -p ~/.local/share/coach
cp assets/profile.template.md ~/.local/share/coach/profile.md
cp assets/flags-log.template.md ~/.local/share/coach/flags-log.md
```

## How it works

Three modes:

| Mode | Trigger | Behavior |
|---|---|---|
| **Passive** (default) | always on | Flags material traps inline, as decisions happen |
| **Challenge** | `coach check`, `grill me on X`, "what am I missing" | Interrogates one plan, ends in a verdict: proceed / proceed-with-changes / don't |
| **Review** | `coach review`, weekly, or at project gates | Audits projects, scores past flags, updates calibration |

Every flag is one line, with three parts — the trap, the cheaper alternative, and the decision that
belongs to you:

```
⚠️ Flag: this is the third setup move this week and nothing is built — cap structure here and ship
the smallest artifact — add it or park it?
```

Severity gates the behavior:

- `🛑 Stop` — irreversible (data deletion, real money, account risk): confirm before acting
- `⚠️ Flag` — expensive but reversible: state it, then continue the work
- `• Note` — low stakes: one line, max one per reply

### The rules that make it liveable

- **Max 3 flags per reply.** More than that and people stop reading flags entirely.
- **Material costs only** — months, money, focus, data, momentum. Taste and style are never flagged.
- **No lectures, no hindsight, no repeating.** An overridden flag is logged and dropped. Third
  repeat of the same pattern earns exactly *one* escalation citing the log, then silence.
- **Coach never replaces work.** Flag, then execute.

### Calibration — the part most tools skip

Every flag and every user decision is appended to `~/.local/share/coach/flags-log.md` and
`decisions.md` (with a review date). At `coach review`:

- past flags are scored **right / wrong / unclear**
- two wrong flags in a row → the skill raises its own threshold
- a miss that cost a week or money → it lowers the threshold for that specific trap class
- overrides that turned out fine → treated as context, not as an error

Coach tracks whether *it* is any good, and tunes itself accordingly. Fewer than ~5 logged flags are
treated as noise, and it says so.

## Files

```
SKILL.md                        the contract: modes, flag format, thresholds, checklist
references/trap-catalogue.md    ~35 traps × 7 domains, each with signal → why → counter-move
references/flag-format.md       severity spec, worked good/bad examples, challenge-mode question bank
references/review-protocol.md   decision journal, calibration scoring, weekly review, kill-date audits
assets/                         templates for the local state files
```

## Customize

The skill reads your local `~/.local/share/coach/profile.md`. Put your real constraints there:

- your known trap patterns (the skill watches for them specifically)
- **standing constraints it must never flag** (decisions you've already made)
- your time budget, active projects, calibration stats
- `sensitivity: gentle | normal | strict`

The state lives outside the skill folder on purpose — updates to the skill never touch your history.

## Design principles

1. Advisory, never blocking. You are the decision-maker; overrides are logged, not fought.
2. One flag must contain a cheaper alternative. No alternative = not ready to flag.
3. Silence is a feature. A guardian that speaks twice an hour is furniture.
4. Self-correction over confidence. Track accuracy, adjust sensitivity, admit misses.
5. Works with any agent that can read markdown.

## License

MIT
