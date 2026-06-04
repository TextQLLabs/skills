# Recipe: Build the Navigation Tables (Routing Files)

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to build the navigation tables for my ontology. These are the only files in the ontology that are auto-attached to chats - they don't contain definitions or workflows themselves, they contain *trigger -> file* rows that tell Ana where to look when a user says something. Every other file in the ontology is triggered by entries in these nav tables.

Depending on my org, I will end up with either one nav table or several:

- A small org may need only one org-wide nav table.
- A typical org will need one org-wide table for shared content, plus a separate nav table per team or role that points to that team's private files.
- A connector- or API-heavy org will additionally want connector-scoped nav tables that load only when that connector is active.

Please run this as a guided session.

## Step 1 - Decide the nav-table topology

Ask me:
- Do we have team-scoped or role-scoped folders today (or are we planning to)? Examples: `finance/`, `sales/`, `product/`, `engineering/`.
- Do we have parallel definitions of the same metric across teams (the Rule 1 pattern: each team has its own `revenue.md`)? If yes, we definitely need team-scoped nav tables - org-wide auto-attach would leak the existence of team-private files.
- Do we have connectors whose nav should only load when that connector is active in the chat? (Useful for data-source-specific routing.)

Based on my answers, draft a topology. The standard pattern is:

```
shared/
  ROUTING.md             <- auto-attached org-wide; points only to org-wide content
finance/
  ROUTING.md             <- auto-attached for finance role
sales/
  ROUTING.md             <- auto-attached for sales role
product/
  ROUTING.md             <- auto-attached for product role
<connector_name>/
  ROUTING.md             <- auto-attached when that connector is selected
```

Confirm the topology with me before moving on. Stub the files as empty for now; we'll fill them next.

## Step 2 - Inventory what each nav table should point to

For each nav table, ask me to list the files in its scope. If the scope is empty (e.g., the team folder doesn't have content yet), park that nav table and come back to it later.

Group the listed files into rough categories:

- Data sources (databases, warehouses, APIs)
- Business context (definitions, stages, roles)
- Workflows (multi-step processes)
- Scheduled artifacts (playbooks, dashboards)
- Personal context (per-user) - usually pointed to from the org-wide table

## Step 3 - Surface the trigger phrases

For each nav table in scope, ask me: when a user with this role / on this team starts a chat, what are the first 20 phrasings they would use? Examples for a Finance nav table:

- "What's recognized revenue last quarter?"
- "Run monthly close"
- "What's our cash burn?"
- "How is COGS calculated?"

I will give you the rough list. You group similar questions together and propose routing buckets.

## Step 4 - Draft each nav table

Use `templates/routing-nav-table-final-v2.md` as the starting skeleton for each nav table you draft. It already has:

- The scope header at the top
- A reminder that auto-attach must be configured by hand in the UI
- Placeholder mini-tables for definitions, workflows, and data sources
- The always-on rules / "before you query" gate / three rules sections (for the org-wide nav table only - delete those sections when drafting role- or team-scoped nav tables)

For each scope, copy the skeleton and fill in the actual routing rows:

```
| Trigger | Go to |
|---|---|
| "How much revenue?" | `finance/context/revenue.md` |
| "Monthly close" | `finance/workflows/monthly-close.md` |
```

Rules:
- Each row routes to a specific file path. No prose definitions in the nav table itself.
- A nav table should stay short - if it grows past ~30-50 rows, split by sub-area (e.g., `finance/ROUTING.md` references `finance/reporting/ROUTING.md` and `finance/close/ROUTING.md`).
- Triggers should be real phrasings users actually use, not abstract category names.

Show me each draft and let me edit before moving on.

## Step 5 - Add the always-on rules to the org-wide nav table

At the top of `shared/ROUTING.md`, draft a section covering the things that must be true in every conversation:

- Which connectors are available org-wide and what each is for
- Any hard "do not do X" rules (don't write to production, don't share customer names externally, etc.)
- Always-true preferences (org timezone, language, response style)
- A pointer to personal context (how Ana finds the current user's preferences)

Team-scoped nav tables should *not* duplicate these - they inherit from the org-wide table.

## Step 6 - Define the "before you query" gate

Inside the org-wide nav table, draft a mandatory gate that says: before running any SQL or hitting any API, check the relevant nav tables first. Do not write SQL from scratch when there's a documented query for the same question. This is the single most valuable rule in the file - it's what stops Ana from improvising and getting things wrong.

## Step 7 - Decide what auto-attaches where

For each nav table, decide and document:

- **Scope**: org-wide / role / connector / API
- **Who it auto-attaches for**: which group, which role, which connector
- **What it points to**: a one-line summary

Output this as a small table at the top of each nav table file, so a future admin can see how it's configured at a glance:

```
> Scope: Finance role
> Auto-attached for: finance, finance-admin
> Points to: files inside finance/
```

> **Critical**: Ana cannot apply the auto-attach configuration itself - file-level properties (which role / API / connector triggers auto-attach for a given file) are admin actions that must be configured by hand in the TextQL app UI, for security reasons. This recipe produces the files and tells you which scope each one is for; you (or an admin) then go into the UI and apply the actual auto-attach binding before the nav table becomes effective.

## Step 8 - Add the three rules to the org-wide nav table footer

At the bottom of `shared/ROUTING.md`, restate the three rules so they ride along in every chat:

1. **One source of truth per scope.** Each team's canonical definition is canonical *for that team*; do not paraphrase across teams.
2. **Routing tables over prose.** The nav tables route; the linked files explain.
3. **Triggered, not always-attached.** Only nav tables auto-load. Everything else is pulled in by trigger or by link.

## Step 9 - Assemble and review

Produce the final set of nav table files. Walk me through each one:

- Is the topology right (org-wide + role-scoped + connector-scoped where needed)?
- Are buckets merged or split correctly?
- Are phrasings realistic for the audience of that nav table?
- Are there missing rules?
- Are most-common triggers at the top?
- Is the auto-attach metadata clearly documented at the top of each file?

Then produce the patch.

## Step 10 - Remind me of the manual UI step

After the patch is produced, remind me explicitly: I need to open the TextQL app UI and configure the auto-attach binding for each nav table file. Until I do, Ana will not actually load these files automatically and the routing won't work in practice.

## Step 11 - Set the next step

Once the nav tables are in (and configured in the UI), recommend what to document next. The right second move is usually one of:

- Fill in the most-frequently-routed-to file (where most of the trigger arrows point)
- Build the routing README for the top-level folder that has the most child files
- Document the slice my team gets wrong most often

Do not just produce the files silently. Walk me through each step and let me steer.
