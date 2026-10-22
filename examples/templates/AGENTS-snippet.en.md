> [中文](AGENTS-snippet.md) | English

# AGENTS.md Snippet (multi-AI collaboration / handover log)

> Add the block below to your workspace `AGENTS.md` (or a project-level `AGENTS.md`).

## Multi-task and multi-AI collaboration

### How to "negotiate"

Agents cannot chat directly with each other; **negotiating = reading and writing the same file**:

1. **Before starting**: read `当前状态.md` (one-page board) + `交接记录.md` (recent entries) first, to learn what is happening now and what pitfalls the previous person left.
2. **Before wrapping up**: add one entry at the **top** of `交接记录.md` (do not append to the end).
3. **Fixed format for each entry**: goal / changes or output (paths) / verification / rollback / next step / details link.
4. **Keep each entry ≤15 lines**; put long logs, command output, and big tables in the project folder or deliverables directory — leave only conclusions and links here.
5. **Roll to keep the most recent 4~10 entries**; move older ones into `交接归档/YYYY-MM.md`, and update the "archive index" in the main file.
6. **No "read-all then rewrite-all"**: reading a large file truncates it; use `edit` to insert at the top, or a script to concatenate lines.
7. **Only one session may write a given file at a time**; notify the other side after writing.
8. For anything involving history/projects/handover, **route first, then read**: run the records router script and read only the matched sections (if `tools/records` is enabled).

### Safety

- Do not run `pnpm add` in a profile for core packages with peer dependencies;
- Do not change the permissions or integrity labels of the workspace or its subdirectories;
- Back up before changing config, and log the change in the version record.
