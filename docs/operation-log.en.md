> [中文](operation-log.md) | English

# Multi-AI Collaboration: The Handover-Log Protocol (one-page board + rolling log + archive)

> Use case: **one workspace** where several AI sessions (or several people plus several AIs) work on the same batch of projects at the same time.
> Goal: let a "later AI" know within 30 seconds **what is happening now, what the previous person changed, and how to roll it back** — while **not stuffing all history into the context**.

---

## 1. The problem: AIs do not share context automatically

What DSH provides natively is **single-session** history and compression; there is **no** cross-session "handover log / shared memory". When several sessions each do their own thing, you get:

- A changes the config, B does not know and changes it back;
- The same file is written by two sessions at once, and the later write overwrites the earlier one;
- A new session steps into the same pitfall an old one already hit;
- One person is forced to relay messages in between.

A real lesson: early on, handover logs were updated by "read-all then rewrite-all", the read tool truncated the file, and **one overwrite lost 9 entries**.

## 2. The solution: three layers of files, each covering one time scale

```text
<workspace>/
├─ 当前状态.md          ← one-page board: what is happening now (ever only "now")
├─ 交接记录.md          ← most recent 4~10 entries (rolling window, newest on top)
└─ 交接归档/
   ├─ 2026-10上旬.md    ← older history (preserved as-is, split by month/ten-day period)
   └─ 2026-11.md
```

| File | When to read | What to write | What not to write |
|---|---|---|---|
| `当前状态.md` | Every time you start work (30 seconds) | Projects in progress, next steps, risks awaiting confirmation | Historical details |
| `交接记录.md` | Start + end of work | What changed this time, how it was verified, how to roll back | Long logs, big tables |
| `交接归档/*.md` | When you need to look up old records (route first, then read) | Historical entries preserved as-is | Do not hand-edit old content |

## 3. What one handover entry looks like

**Before wrapping up, insert it at the top of `交接记录.md`** (do not append to the end), with 6 fixed fields, ideally ≤15 lines:

```markdown
## 2026-10-22 19:50 · assistant name · one-line title

- **Goal**: what this session is meant to solve
- **Changes/output**: which files were changed (with paths), what was produced
- **Verification**: what commands/tests were run, and with what result (don't just write "done")
- **Rollback**: how to undo it (which file to delete / which backup to restore)
- **Next step**: the to-do left for the next person
- **Details**: link to the project folder / long document
```

An anonymized example:

```markdown
## 2026-10-22 19:50 · assistant-b · fix search backend not taking effect

- **Goal**: `web_search` was not using the expected backend
- **Changes/output**: added a `- id: web` config override in `profiles/<profile>/cordis.patch.yml`
- **Verification**: the backend marker appears in real search results; `load-trace` has the registration record; existing features unaffected
- **Rollback**: delete that config block, or restore `backups/cordis.patch.yml.<time>.bak`
- **Next step**: watch the failure rate for a week, then decide whether to adjust the threshold
- **Details**: `docs/search-router.md`
```

## 4. Rotation and archiving (prevent unbounded file growth)

- The main file **keeps only the most recent 4~10 entries**; move older ones into `交接归档/`;
- Split by "month / ten-day period" (e.g. `2026-10上旬.md`); archive files are append-only, never rewritten;
- When archiving, **verify entry by entry**: compute a SHA-256 for each entry and confirm that "the original entry set = main file + archives" — not one entry missing;
- Keep an **archive index table** in the main file:

```markdown
| Time | File | Entries | Notes |
|---|---|---|---|
| 2026-10-13 ~ 10-14 | `交接归档/2026-10上旬.md` | 30 | Postmortems of past incidents |
```

## 5. Five hard rules (bought with real mistakes)

1. **Newest entry on top**: later readers read by time and never have to scroll to the end of the file.
2. **No "read-all then rewrite-all"**: reading a large file truncates it; use `edit` to insert at the top, or a script to concatenate lines.
3. **Only one session may write a file at a time**: when two AIs write the same file concurrently, the later write overwrites the earlier one; notify the other side after writing.
4. **Details go elsewhere**: long logs, command output, and tables belong in the project folder; the handover entry keeps only conclusions + paths.
5. **Do not write keys or personal data**: handover logs may be shared or archived — variable names are fine, values are not.

## 6. How to adopt it in your workspace (5 steps)

1. Create the files/directories: `交接记录.md`, `当前状态.md`, `交接归档/`;
2. Copy `examples/templates/current-status.en.md` to `当前状态.md` and fill in "now";
3. Use `examples/templates/handover-log.en.md` as the entry template;
4. Add a section to your `AGENTS.md` (see `examples/templates/AGENTS-snippet.en.md`):
   - read `当前状态.md` + the most recent entries before starting;
   - add one entry at the top before wrapping up, ≤15 lines;
   - never read an archive in full.
5. After the first 10 entries, do one archive + index cycle, then repeat monthly.

## 7. When to use a "memory plugin" instead of a handover log

| Need | Recommendation |
|---|---|
| Several AIs/people **collaborating**, needing to know "what others did and how to roll back" | **Handover log** (this protocol) |
| Want the AI to **remember automatically** your preferences/facts/lessons | Auto-memory plugins (e.g. `dsh-auto-memory`, `dsh-meow-memory`, etc.) |
| Want to search a large amount of old documents/archives without reading everything | Records router (see `docs/records-router.md` in the sibling repo `dsh-records-router`) |
| Just continue chatting in the same session | DSH's built-in session resume/compression is enough |

> The three can coexist: memory plugins handle "remembering automatically", the handover log handles "collaboration handover", and the records router handles "looking up old records on demand".

## 8. FAQ

**Q: Why not just use one big `notes.md`?**
A: A single file keeps growing; reading it once is expensive, and `read` truncates it anyway; concurrent writes by multiple AIs are risky. With layers, each layer covers only one time scale.

**Q: Will archiving lose content?**
A: Not as long as you follow "move as-is + per-entry hash reconciliation". Do not rewrite old entries along the way.

**Q: Does `当前状态.md` need a daily update?**
A: Just update it when wrapping up; it only records "now", not history. An outdated board is worse than no board, so keep it short.

---

*Templates: [`examples/templates/handover-log.en.md`](../examples/templates/handover-log.en.md) · [`current-status.en.md`](../examples/templates/current-status.en.md) · [`AGENTS-snippet.en.md`](../examples/templates/AGENTS-snippet.en.md)*
