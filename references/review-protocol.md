# Review Protocol — decisions journal, calibration, weekly review

## State files

| File | Purpose |
|---|---|
`~/.local/share/coach/profile.md` | who the user is, known trap patterns, sensitivities, calibration stats |
`~/.local/share/coach/flags-log.md` | append-only: every flag raised, every override, every outcome |
`~/.local/share/coach/decisions.md` | decision journal: commitments + expected outcome + review date |

Create from `assets/*.template.md` on first use. Never edit history rows — append corrections instead.

## Logging a flag (every time)

Append one row to `flags-log.md` immediately after raising a flag:

```
| date | project | trap id | severity | what the user did (followed / overrode / pending) | outcome later |
```

Mark `pending` if unresolved. This is what makes calibration possible later.

## Decision journal (material commitments only)

Log when the user commits to something expensive to reverse: start/kill a project, spend money,
buy a domain/asset, publish something public, park a project, take on recurring work.

```
### YYYY-MM-DD — [decision]
- Decision: ...
- Expected outcome: ... (a number and a date; "it'll work" is not an outcome)
- Cost if wrong: ...
- Review date: ... (default: 14-30 days)
- Status: open | correct | wrong | mixed
```

The review date is the whole point. A journal nobody revisits is a diary.

## Calibration (the self-correction loop)

At every Review, score each `pending`/unscored flag: **right / wrong / unclear**.

Update stats in `profile.md`:

```
Flags raised: N | right: n1 | wrong: n2 | unclear: n3 | overridden: n4 | override outcomes: ...
Last calibration update: YYYY-MM-DD
```

Sensitivity rules:
- **≥2 wrong flags in a row** → raise the bar: flag only 🛑-level or clearly repeated patterns for a while.
- **A miss that cost ≥1 week or money** → drop the bar for that specific trap id; add it to the "known
  blind spot" list with the evidence.
- **Override that turned out fine** → note it; that trap id is context-dependent for this user.
- **Override that turned out badly** → the flag was right; do not say "I told you so". Reference it
  once only if the pattern repeats.

Accuracy expectations: with fewer than 5 logged flags, treat the stats as noise. Say so if asked.

## Weekly review (also run at project gates)

Time-boxed: 15 minutes, produce decisions, not prose.

1. **Projects** — list every active item. For each: last meaningful progress (date), next concrete
   step, still primary or should be parked?
2. **Open decisions** — any journal entries past their review date? Score them.
3. **Flags** — any pending? Score what's scorable.
4. **Spend** — any money out this week? Was it gated behind evidence?
5. **Gates** — any kill dates or launch dates crossing? Restate them.
6. **One change** — exactly one thing to change next week. One. More than one means none.

Output format:

```
## Coach review — YYYY-MM-DD
Projects: [name: status / next step / parked?]
Decisions scored: [...]
Flags: right X / wrong Y / unclear Z
Gates next: [...]
One change: ...
```

## Kill-date audits (special case)

When a kill date arrives:
1. Pull the original success criteria from the project doc.
2. Compare against measured reality (numbers, not impressions).
3. Verdict: **kill** (move to _Archive + post-mortem) / **extend** (new date, new condition, written) /
   **pivot** (same date, changed approach).
4. No fourth option. "Keep going and see" is a decision to extend without a condition, and it is refused.

Post-mortem template lives with the project (`POST-MORTEM.md`): what was built, what the numbers said,
what would have to be true for the next attempt to work.

## Anti-patterns in review

- Reviewing to feel productive, producing no decisions.
- Scoring flags on vibes ("seemed right").
- Re-opening settled decisions without new evidence.
- Letting a review turn into a rebuild of the plan.
- Skipping the review because "nothing happened this week" — that is itself the finding.
