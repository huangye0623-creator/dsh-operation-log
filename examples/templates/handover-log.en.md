> [中文](handover-log.md) | English

# Handover Log Template

> Usage: copy one entry to the **top** of `交接记录.md`; keep each entry ≤15 lines; put details in the project folder.

## YYYY-MM-DD HH:MM · assistant name · one-line title

- **Goal**: what this session is meant to solve
- **Changes/output**: which files were changed (with paths), what was produced
- **Verification**: commands/tests + results (don't just write "done")
- **Rollback**: which file to delete / which backup to restore
- **Next step**: the to-do left for the next person
- **Details**: link to the project folder or long document

---

## Example

## 2026-10-22 19:50 · assistant-b · fix search backend not taking effect

- **Goal**: `web_search` was not using the expected backend
- **Changes/output**: added a `- id: web` config override in `profiles/<profile>/cordis.patch.yml`
- **Verification**: the backend marker appears in search results; load-trace has the registration record; existing features unaffected
- **Rollback**: delete that config block, or restore `backups/cordis.patch.yml.<time>.bak`
- **Next step**: watch the failure rate for a week, then decide whether to adjust the threshold
- **Details**: `docs/search-router.md`
