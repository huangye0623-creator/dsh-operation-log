> [中文](README.md) | English

# DSH Operation Log (operation log / handover protocol)

> When several AI sessions share one workspace, how do you let a newcomer know within 30 seconds "what is happening now, what the previous person changed, and how to roll it back" —
> while **not stuffing all history into the context**.
>
> Unofficial · Templates are copy-paste ready · Docs CC BY 4.0

## Core design: three layers of files

```text
<workspace>/
├─ 当前状态.md          ← one-page board (only "now")
├─ 交接记录.md          ← most recent 4~10 entries (newest on top)
└─ 交接归档/
   ├─ 2026-10上旬.md    ← older history (preserved as-is)
   └─ 2026-11.md
```

| File | When to read | What to write |
|---|---|---|
| `当前状态.md` | Every time you start work (30 seconds) | Projects in progress, next steps, risks awaiting confirmation |
| `交接记录.md` | Start + end of work | Goal / changes / verification / rollback / next step |
| `交接归档/` | When looking up old records | Historical entries preserved as-is |

## Repository contents

| Content | Description |
|---|---|
| [docs/operation-log.en.md](docs/operation-log.en.md) | The protocol: entry template, rotation and archiving, five hard rules, adoption steps, and how it differs from memory plugins |
| [examples/templates/handover-log.en.md](examples/templates/handover-log.en.md) | Handover log entry template |
| [examples/templates/current-status.en.md](examples/templates/current-status.en.md) | One-page current status board template |
| [examples/templates/AGENTS-snippet.en.md](examples/templates/AGENTS-snippet.en.md) | Collaboration rules snippet to add to `AGENTS.md` |

## Five hard rules (bought with real mistakes)

1. Put the newest entry **on top**;
2. **No "read-all then rewrite-all"** (reading a large file truncates it; this once lost 9 entries);
3. Only one session may write a given file **at a time**;
4. Keep details elsewhere: long logs/tables go into the project folder; the handover entry keeps only conclusions + paths;
5. Never write keys or personal data into the handover log.

## Quick start

1. Copy the templates into your workspace;
2. Add `AGENTS-snippet.md` to your `AGENTS.md`;
3. Add one entry at the top of `交接记录.md` every time you wrap up (≤15 lines);
4. Once you have 10 entries, archive them to `交接归档/YYYY-MM.md` and update the index.

## How it differs from "memory plugins"

- Operation log: **collaboration handover** (who changed what, how to roll back);
- Auto-memory plugin: **lets the AI remember** preferences/facts automatically;
- Records router: **looks up archives on demand** (see the sibling repo `dsh-records-router`).

The three can coexist.

## License

Docs CC BY 4.0; example code MIT ([LICENSE](LICENSE)). Unofficial; see [LEGAL.md](LEGAL.en.md) for details.

---

*Summary: an operation-log/handover protocol for multiple AI sessions sharing one workspace — a one-page status board, a rolling handover log, and monthly archives, with templates and five hard-won rules. Docs CC BY 4.0.*
