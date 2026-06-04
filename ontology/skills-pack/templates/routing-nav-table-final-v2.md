# ROUTING.md - <Scope Name>

> Scope: <org-wide | role: \<role\> | team: \<team\> | connector: \<connector\>>
> Auto-attached for: <list of roles, groups, or connectors this auto-attaches to>
> Points to: files inside <folder path>

> **Auto-attach must be configured by hand in the TextQL app UI.** Ana cannot apply this binding. Until an admin sets it, this file is not actually loaded automatically.

---

## Always-on rules (org-wide ROUTING.md ONLY - delete this section if role/team/connector scoped)

- Org timezone: <timezone>
- Available connectors org-wide: <list>
- Hard "do not do X" rules: <list>
- Pointer to personal context: Ana reads `users/<email-local-part>/context.md` at the start of every chat

---

## Routing tables

Each row routes a trigger phrase to a specific file path. No prose definitions in this file - just routes.

### <Mini-table 1: e.g., Definitions / Concepts>

| Trigger | Go to |
|---|---|
| "<phrase>" | `<path/to/file.md>` |
| "<phrase>" | `<path/to/file.md>` |

### <Mini-table 2: e.g., Workflows>

| Trigger | Go to |
|---|---|
| "<phrase>" | `<path/to/file.md>` |
| "<phrase>" | `<path/to/file.md>` |

### <Mini-table 3: e.g., Data sources / queries>

| Trigger | Go to |
|---|---|
| "<phrase>" | `<path/to/file.md>` |
| "<phrase>" | `<path/to/file.md>` |

---

## The "before you query" gate (org-wide ROUTING.md ONLY)

Before running any SQL or hitting any API, check the routing tables above. If a canonical query exists for the question, use it rather than writing fresh SQL.

---

## The three rules (org-wide ROUTING.md ONLY)

1. **One source of truth per scope.** Each team's canonical definition is canonical *for that team*; do not paraphrase across teams.
2. **Routing tables over prose.** ROUTING.md files route; the linked files explain.
3. **Triggered, not always-attached.** Only ROUTING.md files auto-load. Everything else is pulled in by trigger or by link.

---

## Maintenance notes

- Keep this file short. If it grows past ~30-50 routing rows, split by sub-area (e.g., this file points to `<sub_area>/ROUTING.md`).
- When a new file is added to this scope, add a routing row here for it.
- When a file is removed or renamed, update or remove its row here.
