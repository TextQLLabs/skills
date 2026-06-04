# Recipe: Decide the Folder Structure

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to decide on the folder structure for my ontology. Please run this as a guided session - do not generate a final structure until we've worked through the trade-offs together.

## Step 1 - When do I need folders at all?

Ask me how many files I currently have. If I have fewer than 10, recommend staying flat for now. Folder structure is a tool for managing scale, not a starting design.

If I have more than ~15 files, walk me through the next steps.

## Step 2 - Walk me through the standard top-level categories

Most ontologies end up with some subset of these top-level folders. Walk me through each one and ask whether I need it.

> Inside a data source or team folder, you'll typically see a mix of `.tql` (objects, metrics, composed views) and `.md` (READMEs, gotchas, workflows, definitions), plus any supporting `.py`, `.csv`, `.xlsx`, `.html`, or image files. Folder structure is about how to *group* these; the file types coexist.


| Top-level folder | What goes here | Skip if... |
|---|---|---|
| `databases/` (or `data_sources/`) | Schemas, table docs, query catalogs for every database/warehouse | I only have one data source and one or two queries |
| `apis/` | External API integration guides, auth patterns, write-safety rules | I only call one API and barely |
| `<functional_area>/` (e.g., `go_to_market/`, `finance/`, `engineering/`) | A team or functional hub: business context + workflows + visualizations for one area | My ontology is single-team or cross-functional throughout |
| `users/` | Per-user personal context (preferences, typos, workflows) | I'm the only user and my context fits in one file |
| `playbooks/` | Scheduled automated analyses with a mission doc | I don't run any scheduled reports yet |
| `dashboards/` | Dashboards (often code + README per dashboard) | I don't build dashboards in-platform |
| `datasets/` | Static CSVs, exports, snapshots that aren't in a live database | I don't have any |
| `skills/` (or `tools/`) | Reusable code skills Ana can import | I don't have any custom code skills yet |
| `templates/` | Document skeletons referenced by other docs | I'm not yet at the stage of formalizing templates |

Bias toward fewer folders. **Every folder you add must have a README that justifies its existence.**

## Step 3 - The personal context placement question

This is a frequent source of confusion. Walk me through the options:

**Option A: Top-level `users/` folder (recommended default).**

```
users/
  README.md
  <user_1>/
    context.md
  <user_2>/
    context.md
```

Pros: One canonical home for "how Ana should interact with each person." Easy to keep separate from "facts about people for others to read." Easy to back up or migrate.

Cons: All users in one folder regardless of team.

**Option B: Nested under each team (sometimes requested).**

```
finance/
  users/
    <user_1>/
      context.md
engineering/
  users/
    <user_2>/
      context.md
```

Pros: Each team owns its own people.

Cons: If a person belongs to multiple teams, where does their context go? If they switch teams, the file moves and links break. Personal context is about *the person*, not about their current team.

**Recommendation:** Use Option A (top-level `users/`). Keep team/role information *inside* the user's `context.md` file (e.g., "I am on the finance team"), not in the folder path.

Confirm with me before proceeding.

## Step 4 - The parallel-definitions question (critical)

Before talking about sensitive content, talk about *team-scoped definitions*. This is a third of the point of ontology and folder structure is what makes it work.

Ask me:
- Are there metrics or concepts that different teams in my org define differently? Examples to probe:
  - **Revenue** - Sales (closed-won), Finance (recognized), Product (consumption)?
  - **Active customer** - Sales (recent deal), Product (recent event), Finance (currently billing)?
  - **Churn**, **headcount**, **margin**, **conversion**, **engagement** - same problem in most orgs
- For each one, can the same person be both right and wrong depending on which team's hat they're wearing?

If yes (it almost always is yes), each team needs its own canonical definition file, gated to that team's folder. The folder structure is the access model:

```
finance/
  context/
    revenue.md           <- recognized revenue (GAAP)
sales/
  context/
    revenue.md           <- closed-won pipeline value
product/
  context/
    revenue.md           <- consumption dollars
shared/
  reconciliation/
    revenue-bridge.md    <- explains how the three relate (optional, if teams need to compare)
```

Folder-level RBAC keeps each team's definition isolated. Ana reads only what the asker has access to, so a Sales rep asking "what was revenue last quarter" gets the Sales answer, not the Finance one - automatically, no prompt engineering needed.

Decide now, for the top 3-5 metrics my team cares about most, which ones need this treatment. Don't try to enumerate all of them. Get the top ones right and add as you go.

## Step 5 - Security and visibility considerations

In addition to the team-scoping above, ask whether any of the following apply:

- **Sensitive business areas** (compensation, M&A, board materials, legal): dedicate a folder with explicit access notes at the top of each file. Do not let these leak into general routing tables.
- **Customer-specific information** (client names, deal values, account details): standardize on placeholders (`<Customer A>`) in the ontology; keep live values in the CRM or warehouse itself, not in `.md` files.
- **PII or compliance-sensitive content**: do not store in the ontology at all. Reference where it lives ("this lives in <system>, access via <process>"); don't replicate.
- **Cross-organization sharing**: if any subset of the ontology is intended to be shared externally (with a partner, customer, auditor), keep that subset in a clearly labeled folder and review every file in it before sharing.

Each of these boundaries deserves a one-paragraph rule at the top of the affected folder's README.

## Step 6 - Visualization placement

Walk me through where visualizations should live:

- **PDF generators (one-off reports):** Co-locate with the workflow they support. Example: `<functional_area>/workflows/visualizations/`.
- **Streamlit / interactive dashboards (recurring, embedded in-app):** Their own top-level folder (`dashboards/`) because they're a different artifact type.
- **Slides and presentations:** A `skills/slides/` folder if your tooling allows Ana to generate them; otherwise reference the templates that live in your design tool (Figma, Google Slides, etc.).
- **Brand assets (logos, color tokens, fonts):** Reference where they live (e.g., a GitHub repo, a Drive folder). Don't replicate.

## Step 7 - Draft the structure

Based on my answers, draft a folder tree. Show it to me as ASCII. For each folder, include a one-line justification. Always include the `shared/` folder for org-wide content, and always include a `ROUTING.md` file at the root of every folder that is intended to auto-attach for a specific scope (org-wide, role, team, or connector):

```
shared/                  - Org-wide content readable by everyone
  ROUTING.md             - Auto-attached org-wide; routes to other org-wide files
  README.md              - Human-readable index of what's in shared/
  context/               - Org-wide definitions (use sparingly; prefer team-scoped)
databases/               - One folder per database/warehouse our team queries
  README.md              - Index of databases
  <source>/
    ROUTING.md           - Auto-attached when that connector is selected
    README.md            - Human overview of the source
apis/                    - One folder per external API we integrate with
  README.md              - Index of APIs
  <service>/
    README.md            - Human overview (auth, rate limits, routing rule)
finance/                 - Finance team hub: definitions, workflows, reports
  ROUTING.md             - Auto-attached for the finance role
  README.md              - Human overview of the finance hub
  context/               - "What does X mean" docs
  workflows/             - "How do we do X" docs
  reports/               - PDF/sheet templates and generators
sales/                   - Sales team hub (parallel structure to finance/)
  ROUTING.md             - Auto-attached for the sales role
  README.md
  context/
  workflows/
users/                   - Per-user personal context
  <user_local_part>/
    context.md           - That user's preferences, typos, workflows
playbooks/               - Scheduled automated analyses
templates/               - Skeletons referenced by other docs
```

Notes:
- `ROUTING.md` is the auto-attached spine. Only folders whose content should *load automatically* for some scope need one.
- `README.md` is the human entry point. Every folder gets one regardless of whether it has a `ROUTING.md`.
- `shared/` is the home for content that genuinely applies to everyone in the org. Most metric-related content does NOT belong here - see Rule 1 in the start-here guide on parallel definitions per team.

## Step 8 - Per-folder README.md and ROUTING.md rules

The two file types serve different audiences. Set the expectation now so they don't get conflated.

**`README.md`** - Human reference. NOT auto-attached. Lives in every folder. Three jobs:

1. State the folder's purpose in one sentence
2. List its child files/folders with a one-line description each
3. Provide a small "Quick routing" table for common phrasings as a convenience for humans browsing the tree

A README is what a new team member reads when they're orienting themselves.

**`ROUTING.md`** - Machine nav. Auto-attached for a defined scope (org-wide, role, team, or connector). Lives only in folders whose content is meant to *load automatically* into chats matching that scope. Three jobs:

1. A scope header at the top documenting who this file auto-attaches for (e.g., `Scope: Finance role; Auto-attached for: finance, finance-admin`)
2. One or more `trigger phrase -> path/to/file.md` routing tables
3. Optionally, a short "always-on rules" section in the org-wide `shared/ROUTING.md` only (do not duplicate these in role-scoped routing files)

A ROUTING.md is what Ana reads at the start of every chat in its scope. It must stay small - if it grows past ~30-50 rows, split into sub-area routing files.

**Rule of thumb:** every folder gets a README. Folders that need to auto-attach also get a ROUTING.md alongside the README. The two files coexist; they are not substitutes.

> **Manual UI step**: As noted in the start-here guide, Ana cannot apply the auto-attach binding for a `ROUTING.md` file. After this session, an admin has to open the TextQL app UI and configure which role / scope / connector each ROUTING.md auto-attaches for, before the routing takes effect.

## Step 9 - Patch and roll-out plan

Produce the patch that creates the folders (with empty README stubs). Then recommend the order in which to fill them:

1. Start with the folder that has the most child content already
2. Fill in business context before workflows (workflows reference context)
3. Fill in data source docs before queries (queries reference tables)
4. Leave `templates/` until you have 3+ files that look enough alike that a template is obvious

Do not generate the structure silently. Walk me through and let me steer.
