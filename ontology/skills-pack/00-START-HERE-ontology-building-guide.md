# Ontology Building Session - Start Here

> A guide to building your organization's ontology with Ana, one slice at a time, by dropping recipe files into chat.

---

## Step 1 — Ask first, then fetch only what you need

**When this file is in the chat, do not start building yet.** Ask the user:

> "What do you want to build or document today?"

Based on their answer, tell them exactly which files to download from the kit at `https://github.com/TextQLLabs/skills/tree/main/ontology/skills-pack`. They only need the files for their task — not the whole repo.

| What the user wants to do | Recipe file(s) to download | Template file(s) to download |
|---|---|---|
| Not sure / exploring from scratch | `recipes/recipe-00-scoping-conversation.md` | — |
| Build `.tql` files for metrics or queries | `recipes/recipe-11-tql-authoring.md` | `templates/tql-file.md` |
| Document a database or data source | `recipes/recipe-03-database-or-data-source.md` | `templates/database-readme.md`, `templates/table-doc.md`, `templates/query-catalog.md` |
| Document an API integration | `recipes/recipe-04-api-integration.md` | `templates/api-readme.md` |
| Document a repeated workflow | `recipes/recipe-05-workflow-doc.md` | `templates/workflow-doc.md` |
| Define a business concept, metric, or stage | `recipes/recipe-06-business-context-doc.md` | `templates/business-context-doc.md` |
| Build a personal context profile | `recipes/recipe-07-personal-context.md` | `templates/personal-context.md` |
| Build a team or functional area hub | `recipes/recipe-08-team-or-functional-area.md` | `templates/folder-readme.md`, `templates/routing-nav-table.md` |
| Build routing / nav tables (ROUTING.md) | `recipes/recipe-01-nav-tables.md` | `templates/routing-nav-table.md` |
| Decide folder structure | `recipes/recipe-02-folder-structure.md` | `templates/folder-readme.md` |
| Document a playbook or dashboard | `recipes/recipe-09-playbook-or-dashboard.md` | `templates/playbook-mission.md` |
| Reference file anatomy / review a draft | `recipes/recipe-10-file-anatomy-template.md` | — |

Once you have identified the right files, tell the user:

> "Download those files from the repo and drop the **recipe file** into a new chat to start your guided session. The **template file(s)** are the skeletons Ana will use when drafting your content — include them in the same chat."

The rest of this file is background context. Read it if you want to understand how the system fits together, but the session itself happens in the next chat with the recipe file.

---

## What ontology is

Ontology is Ana's unified, living memory of your business - one git-backed repository holding every definition, metric, script, document, dataset, dashboard, and asset Ana uses to reason about your data. It's the layer that lets Ana answer questions in *your* business's terms, not just your warehouse's table names. The same repository that teaches Ana what "active customer" means also gives Ana the brand chart styles to use when plotting them.

Ontology used to be two separate systems: a semantic layer for structured modeling, and a context library for unstructured documentation. Ontology 3.0 unifies them. Structured and unstructured context are more useful when they can cross-reference each other - a metric definition can link to the meeting notes where it was decided, a workflow can pull in the SQL it depends on, a dashboard helper can reference the brand palette.

The ontology holds whatever Ana might need:

- **Semantic models in `.tql`** - TextQL's SQL-native modeling language for objects, attributes, links between objects, metric definitions, and composed query views. Compiles to warehouse-native SQL. (`recipe-11-tql-authoring.md` teaches the syntax with copy-pasteable templates (condensed from the official [.tql Manual](https://docs.textql.com/core/ontology/tql-reference), which is the source of truth); `recipe-03` covers what to model.)
- **Unstructured context in `.md`** - canonical definitions, workflow checklists, business rules, role descriptions, meeting notes, gotchas, conventions, and anything else that won't fit cleanly in code.
- **Code and scripts in `.py`** - dashboard logic, custom metric implementations, reusable skills.
- **Data files in `.csv`, `.tsv`, `.xlsx`, `.parquet`** - static reference data, exports, snapshots, forecast inputs.
- **Reports and visuals in `.html`, `.png`, `.svg`, `.pdf`, `.pptx`** - dashboard fragments, diagrams, brand assets, slide templates that Ana uses to produce pixel-perfect deliverables that match your company's format.
- **Configuration in `.yaml`, `.json`** - targets, quotas, mappings, settings.

Everything Ana can use, you can store. Everything in here is versioned, permissioned by folder (RBAC, SCIM, SSO inherited from your org), and accumulates value over time - the more your team uses Ana, the cheaper and faster each query becomes as paths get reused.

Because everything lives in files and folders, Ana navigates it three ways: by keyword when there's a term to look for, by meaning when there isn't, and by tracing references between files. A question about churn pulls in how churn is defined, the `.tql` that calculates it, the events it depends on, the cohorts it filters against, the definition of "active" underneath, and the chart style to render it in - all from following links.

---

## The method: principles, tracks, and inputs

Five principles underpin everything in this kit. (The three rules further down are how you *operationalize* them.)

1. **Just files.** Definitions, SQL, notes, and rules live in one versioned tree - portable, diff-able, yours. No black-box compiler.
2. **Locality of behavior.** You understand a metric by reading its file and the few it imports. `.tql` renders inspectable, warehouse-native SQL.
3. **Governed by default.** Changes are proposed -> diffed -> reviewed -> approved -> audited, like software.
4. **Ana does the heavy lifting.** Ana inspects the `.tql`, renders the SQL, executes it, and prefers a governed surface over ad-hoc SQL.
5. **Discovery first.** Build from what's actually in the schema and the documents, not from assumptions.

### Three ways to start

- **From a prebuilt domain template** - fastest, if one fits your domain (e.g. healthcare/life-sciences, financial services). Connect it, validate against your schema, adapt. Run `recipes/recipe-11-start-from-domain-template.md`.
- **From a database** (discovery-first). Connect it; Ana profiles the schema and drafts the first objects and metrics. Run `recipes/recipe-03-database-or-data-source.md`.
- **From a pile of documents** (document-first). SOPs, metric docs, transcripts, policies - Ana reads them and proposes the model, then you slice it with `recipe-05` / `recipe-06`.

Then, on any path: **validate** (`recipe-12-validate-with-golden-queries.md`), **govern** (`recipe-13-role-based-access.md`), and **maintain** (PRs - see the end of this guide).

### Where each kind of input lands

Real organizations have messy inputs. Each kind has a home in the ontology:

| Input | Lands as |
|---|---|
| Business docs (SOPs, policies, glossaries) | `.md` context + canonical definitions (`recipe-06`) |
| Metrics documentation | governed `.tql` surfaces + a definition note (`recipe-03`/`recipe-06`) |
| Database + data dictionaries | schema backings + objects + table docs (`recipe-03`) |
| Process-flow diagrams | objects + relationships + a workflow doc (`recipe-05`) |
| Call transcripts / unstructured text | terminology, intents, entities -> glossary + definitions |
| Golden datasets | golden-query tests (`recipe-12`) |
| Access / sensitive policies | role-based access + governed notes (`recipe-13`) |

> **Worked example.** For a full before/after - a pile of mixed inputs turned into a built ontology - see the `example-scenario/` in the ontology-workshop-guide: https://github.com/TextQLLabs/ontology-workshop-guide/tree/main/example-scenario

---

## How this kit is organized

```
00-START-HERE-ontology-building-guide.md   <- you are here
recipes/
  recipe-00-scoping-conversation.md        <- start here in chat
  recipe-01-nav-tables.md           <- the master nav for the repo
  recipe-02-folder-structure.md
  recipe-03-database-or-data-source.md
  recipe-04-api-integration.md
  recipe-05-workflow-doc.md
  recipe-06-business-context-doc.md        <- canonical definitions
  recipe-07-personal-context.md
  recipe-08-team-or-functional-area.md
  recipe-09-playbook-or-dashboard.md
  recipe-10-file-anatomy-template.md
  recipe-11-tql-authoring.md               <- how to actually write .tql (syntax + templates)
templates/
  folder-readme.md
  routing-nav-table.md
  database-readme.md
  table-doc.md
  query-catalog.md
  api-readme.md
  workflow-doc.md
  business-context-doc.md
  personal-context.md
  playbook-mission.md
  tql-file.md                              <- .tql skeletons (query / composed view / _defs)
```

**Recipes** are prompts. You drop the contents of one into chat and Ana runs a guided session for that slice of the ontology.

**Templates** are skeletons. Recipes will pull from them when generating the actual files.

---

## How a session works

Each session is short and focused. The pattern is the same every time:

1. **You drop a recipe into chat.** Copy the contents of one `recipes/*.md` file as your message.
2. **Ana asks a small number of scoping questions.** Usually 3-7. Answer as you can; "I don't know yet" is a fine answer.
3. **Ana drafts the file(s).** As a pull request you can review.
4. **You review and edit.** Push back, correct, fill in missing pieces.
5. **You approve the PR.** The change merges into your ontology and becomes available to everyone with access.

A typical session is 10-30 minutes per slice. The goal of one session is one well-formed file or a small set of related files - not the whole ontology.

This is the same flow Ana uses on its own. As your team queries the warehouse, Ana proposes ontology updates through PRs when something useful is learned. You review, edit, and merge. The knowledge graph improves continuously without anyone losing oversight.

---

## Before your first session: gather inputs

You don't need to format anything. Raw SQL, Notion links, Slack exports, screenshots of dashboards - drop them in. The goal is to start with signal, not structure. That said, the slices below are accelerated by having the right material on hand.

**For a data source slice (recipe-03):**
- Connector name or warehouse credentials your team uses
- The top 5-15 tables your team queries
- One or two recurring queries written as SQL
- Any "everyone gets this wrong" gotchas (timezone, soft-deletes, internal/test data exclusions)

**For an API slice (recipe-04):**
- Auth method and base URL
- An example of one read and one write your team does
- Rate limits or "must do X first" rules

**For a workflow slice (recipe-05):**
- Walk yourself through the workflow slowly while writing down every step
- Identify which steps touch which systems (CRM, warehouse, Slack, email, spreadsheet)
- Note any "if X then Y" branches

**For a business context / canonical definition (recipe-06):**
- The term, its 2-3 most common synonyms or misspellings
- The canonical definition in one sentence
- The 3-5 things people commonly get wrong about it
- Examples and counterexamples

**For a `.tql` authoring slice (recipe-11):**
- The tables/columns the file will touch and what one row represents
- The inputs the file should accept (which are required, which have defaults)
- Whether the file needs conditional logic (optional joins, breakout dimensions) or is a single static SELECT
- Access to the connector so the file can be tested end to end (inspect -> render -> execute)

**For personal context (recipe-07):**
- How you prefer responses (concise vs detailed, charts vs tables, with-SQL vs without)
- Your common typos and shorthand (especially if you dictate)
- The tools, dashboards, and reports you use weekly

---

## Pick a starting slice

Resist the urge to build everything at once. Pick one:

| If you have... | Start with... |
|---|---|
| One database your team queries every day | `recipe-03-database-or-data-source.md` for that database |
| One API integration your team relies on | `recipe-04-api-integration.md` for that API |
| A workflow that gets explained over and over in Slack | `recipe-05-workflow-doc.md` for that workflow |
| A term people argue over the definition of | `recipe-06-business-context-doc.md` for that term |
| Nothing yet, just exploring | `recipe-00-scoping-conversation.md` |
| A query you want to turn into a reusable, parameterized `.tql` | `recipe-11-tql-authoring.md` |

Once 3-4 slices exist, build the **root routing file** (`recipe-01-nav-tables.md`). This is your repo's nav.

Once you have ~10-15 files, decide whether to introduce **folder structure** beyond flat (`recipe-02-folder-structure.md`).

---

## The three rules

These show up everywhere in this kit. Internalize them.

### Rule 1: One source of truth per scope - everywhere else points

Inside a single scope (a team, a function, a folder), every concept has exactly one canonical definition. Every other file in that scope that mentions the concept links to it. Do not redefine. Do not paraphrase. Link.

When the definition changes, you change it in one place and the entire scope updates by reference.

**Different teams will define the same metric differently. That is correct and expected.** Sales counts "revenue" as closed-won pipeline value. Finance counts "revenue" as recognized revenue per GAAP. Product counts "revenue" as consumption-driven usage dollars. All three are right - for their team. The ontology supports this directly: each team owns its own folder with its own canonical definition, and folder-level RBAC ensures Ana sees only the definitions the asker has access to.

This is roughly a third of the point of ontology. Without it, the only way to reconcile "what was revenue last quarter" across teams is a meeting; with it, each team gets a high-fidelity answer in their own terms, and the meeting only happens when teams genuinely need to compare.

**How to set this up:**

1. **One canonical file per team per concept.** `finance/context/revenue.md`, `sales/context/revenue.md`, `product/context/revenue.md`. Each one is the single source of truth *for that team*. The matching `.tql` definitions live in the same scope (e.g., `finance/objects/revenue.tql`).
2. **Gate at the folder level.** Permission `finance/` so only finance can see it, `sales/` so only sales can see it, etc. Ana inherits these permissions automatically - it cannot return a definition the asker is not allowed to see.
3. **At the org root, point but do not define.** The root routing file should route "revenue" questions to the team-specific definition based on who's asking, not pick one canonical definition for the whole org. A small disambiguation note ("revenue means different things in different teams - see your team's folder") is enough.
4. **For shared definitions, use a `shared/` folder.** Concepts that genuinely are the same everywhere (e.g., "fiscal year start date", "company holidays") go in a folder all teams can read. Use this sparingly - most "obviously shared" concepts turn out to have team-specific nuance.
5. **When two teams need to reconcile their definitions, write a third file.** `finance/context/revenue.md` and `sales/context/revenue.md` can both link to `shared/reconciliation/revenue-bridge.md`, which explains how the two definitions relate and why they differ. That bridge file is itself a canonical source - just for the *relationship* between the two definitions.

The discipline is: one source of truth *per scope*, not one source of truth *period*. Multiple correct answers can coexist, and RBAC is what makes that safe.

### Rule 2: Routing tables over prose

The most useful page in any directory is a small table at the top:

| Trigger phrase | Go to |
|---|---|
| "How much revenue did we book?" | `path/to/revenue.md` |
| "What's our pipeline?" | `path/to/pipeline.md` |

Prose explanations belong inside the linked files, not in the index. The index's job is to route.

### Rule 3: Triggered, not always-attached - with a nav table that *is* attached

Most files in the ontology should not be loaded into every conversation. They are **discoverable through routing tables and pulled in only when relevant**. Everything Ana needs is reachable, but only the routing tables themselves ride along in every chat.

The mechanism that makes this work is a **navigation table inside a file that is auto-attached**. The nav table is the index - it lists "when the user says X, pull in `path/to/file.md`" rows for every triggerable file in its scope. The file that contains the nav table is the only thing always loaded; every file *it* points to gets pulled in on demand when Ana sees a matching trigger.

**Scope the auto-attach correctly:**

- **Org-wide auto-attach** for a nav table that points only to files everyone in the org can read. If the table itself references team-private files, do not auto-attach it org-wide - it would leak the existence of those files to people who shouldn't know they exist.
- **Role- or team-scoped auto-attach** for a nav table that points to files only that role or team can read. Sales gets a Sales nav table auto-attached on their chats; Finance gets the Finance one; an exec with both roles gets both. (Setting which file auto-attaches for which role is the file-level property that has to be configured by hand in the UI - see the Security and access section below.)
- **Connector- or API-scoped auto-attach** for nav tables that route inside a specific data source or API. The connector's nav table loads only when that connector is active in the chat.

**Pattern:**

```
shared/
  ROUTING.md             <- auto-attached org-wide; routes to other org-wide files
finance/
  ROUTING.md             <- auto-attached for the finance role; routes inside finance/
  context/
    revenue.md           <- triggered by "revenue" entries in finance/ROUTING.md
  workflows/
    monthly-close.md     <- triggered by "monthly close" entries in finance/ROUTING.md
sales/
  ROUTING.md             <- auto-attached for the sales role
  context/
    revenue.md           <- triggered when Sales asks about revenue
```

When a finance user starts a chat, Ana loads `shared/ROUTING.md` and `finance/ROUTING.md` automatically. Neither file contains definitions - they only contain trigger -> path rows. When the user says "what was revenue last quarter," the Finance nav table triggers `finance/context/revenue.md` to be pulled in. The Sales `revenue.md` is never loaded for that user because the Sales nav table was never auto-attached for them.

**Keep nav tables small.** A good nav table is short enough that auto-attaching it costs almost nothing. If it grows past ~30-50 rows, split it into sub-tables (one per top-level area) and have the parent nav table point to the sub-tables.

This keeps Ana's context window focused, lets the ontology grow arbitrarily large without slowing every chat down, and means each user only ever sees the slice of the ontology that's relevant to them and their permissions.

---

## Security and access

Permissions follow the folder structure. RBAC, SCIM, and SSO are inherited from your organization. If a user cannot see a folder, Ana cannot see it either when that user is the one asking.

This isn't just for sensitive content - it's also how the ontology supports the parallel-definitions pattern from Rule 1 above. When Sales has its own `revenue.md` and Finance has its own `revenue.md`, the only thing keeping Ana from mixing them up is that each folder is gated to the right group. Folder structure *is* the access model. Plan it accordingly.

> **Important - configure access by hand, in the UI.** For security reasons, Ana cannot configure or manage folder-level access controls, and Ana cannot set file-level properties (the auto-attach rules that bind a file to specific roles, APIs, or connectors). These are admin actions, not agent actions, and they must be done by a human in the TextQL app UI. Ana will help you *design* the access model - which folders, which groups, which roles - and will produce the folder skeletons and `.md` / `.tql` content inside them, but you are the one who has to open the platform settings and apply the permissions, role bindings, and auto-attach configurations after each session. Plan to do this pass at the end of every recipe that creates new folders or files with non-default visibility, before the next session begins. Skipping this step means a folder Ana created today is visible to everyone in the org until you go gate it.

Common boundaries to set up early (each requires the manual UI step above):

- **Team-scoped definitions** (the parallel-definitions pattern): one folder per team, each gated to that team's group.
- **Sensitive business areas** (compensation, M&A, board materials, legal): dedicate a folder, restrict access to the right group, and put an access note at the top of every file in it.
- **Customer-specific data**: prefer placeholders (`<Customer A>`) in scrubbed examples; keep live values in the CRM or warehouse rather than in `.md` files.
- **PII and compliance-sensitive content**: do not duplicate it into the ontology. Reference where it lives and how to access it; don't replicate.
- **Externally-shared content**: keep it in a clearly labeled folder and review every file before sharing.
- **Auto-attached files** (the always-on context for a role, API, or connector): set these sparingly and configure by hand. Auto-attach means the file enters every relevant chat's context, so the cost of mis-configuring is high.

Credentials never go in the ontology. Use a credentials manager. The ontology is a knowledge layer, not a vault.

---

## After the kit: maintaining the ontology

Treat the ontology like code:

- **Pull requests, not direct edits.** Every change is reviewed. Ana proposes; humans approve.
- **Update when the world changes.** If a process changed, update the file the same day.
- **Prune.** Files that haven't been read in 6 months should either be archived or deleted.
- **Personal context gets revised continuously.** Ana should be offering "should I add this to your personal context?" suggestions; say yes when they're useful.

---

## What good looks like

After a few weeks of work, your ontology should:

- Answer a new team member's "where do I find X?" question in one link
- Give Ana the right SQL pattern on the first try
- Survive the departure of any one person on the team
- Drive the cost and latency of repeated queries down measurably as the same paths get reused

You are not building documentation for the sake of documentation. You are building the brain Ana uses to answer questions on your team's behalf.

Pick a recipe and start.
