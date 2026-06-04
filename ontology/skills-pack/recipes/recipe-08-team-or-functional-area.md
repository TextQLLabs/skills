# Recipe: Build a Team or Functional Area Hub

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to build the hub for one team or functional area in my ontology. A hub is a top-level folder that holds business context, workflows, visualizations, and other artifacts for a single domain (e.g., a `finance/` folder, a `go_to_market/` folder, an `engineering/` folder).

Please run this as a guided session.

## Step 1 - Identify the team and its boundary

Ask me:
- The name of the team or functional area
- Who's on it (so Ana knows when a question comes from someone inside this team)
- What kinds of questions belong to this team (vs other teams)
- What systems this team owns or depends on (CRM, billing, ticketing, etc.)
- Whether any topic is *jointly* owned with another team (and how to route in that case)

## Step 2 - Inventory what exists for this team already

Ask me what's currently in the ontology that touches this team. Group it:

- Data sources (databases / APIs the team uses)
- Existing business context docs (definitions, stages, roles)
- Existing workflow docs
- Existing visualizations or reports
- Personal context files of team members

If most of these are currently scattered around the ontology, suggest consolidating them under the team's hub folder.

## Step 3 - Standard hub structure

Walk me through the standard layout. Adapt as needed - not every team needs every sub-folder. **Every team hub has both a `README.md` (human reference) and a `ROUTING.md` (auto-attached for the team's role)** - these are different files with different jobs:

```
<team_name>/
  README.md              <- HUMAN: hub overview, role map, where everything lives. NOT auto-attached.
  ROUTING.md             <- MACHINE: trigger -> file routing table. Auto-attached for the team's role.
  context/               <- business context (definitions, stages, roles)
    README.md            <- index of context docs
    <concept_1>.md
    <concept_2>.md
    ...
  workflows/             <- multi-step processes
    README.md            <- index of workflow docs
    <workflow_1>.md
    <workflow_2>.md
    ...
  visualizations/        <- PDF generators, dashboard code, report templates
    README.md
    <generator_1>.py     <- if your platform supports it
  meetings/              <- recurring meeting notes that are referenced often (offsites, all-hands)
    <date>-<event>.md
  data_quality.md        <- if this team owns a data set with quality issues
```

`README.md` orients a human browsing the folder. `ROUTING.md` is what Ana auto-loads at the start of every chat where the user has the team's role. See the start-here guide's Rule 3 for the full mechanic, and recipe-01 (`recipe-01-nav-tables.md`) for how to draft the ROUTING.md content.

## Step 4 - The hub README and ROUTING.md

The hub has two top-level files, each with a different job.

### `<team_name>/README.md` - human orientation

This is the most-read file in the hub by humans. Standard sections:

1. **Title and one-line purpose** ("This is the <Team> hub. <Purpose>.")
2. **Domain boundary** ("Questions about X belong here. Questions about Y belong in <other hub>.")
3. **Architecture diagram** if the team owns a non-trivial pipeline (ASCII is fine; clarity beats elegance)
4. **Team roles** (who plays which role; link to the canonical role definitions)
5. **Who sees what** (visibility rules - e.g., "Sellers only see their own deals; the CRO sees all")
6. **Read/write architecture** if the team has both read and write paths to a system
7. **Folder map** (small table linking to each sub-folder's README)

The README is not auto-attached. It is read on demand by humans browsing the repo.

### `<team_name>/ROUTING.md` - auto-attached for the team's role

This is what Ana loads at the start of every chat where the user has this team's role. It is small, all routing tables, no prose. Standard sections:

1. **Scope header** at the top:
   ```
   > Scope: <Team> role
   > Auto-attached for: <role>, <role-admin>
   > Points to: files inside <team_name>/
   ```
2. **Trigger -> file routing tables** for the top 20-30 questions a user with this role would ask. Group by area (one mini-table for context docs, one for workflows, etc.).
3. **No always-on rules** - those live in `shared/ROUTING.md` only and are inherited.

Draft each ROUTING.md by working through `recipe-01-nav-tables.md` once per team hub, or do it inline here as a single step if the topology is small. Either way, do not skip producing the ROUTING.md - without it, the hub's content is not auto-discoverable for the role.

> **Manual UI step**: As noted in the start-here guide, Ana cannot bind a ROUTING.md to its role. An admin has to open the TextQL app UI and configure the auto-attach for `<team_name>/ROUTING.md` against the team's role(s) before the routing takes effect.

## Step 5 - The context/ folder

This holds "what does X mean" docs. One file per concept. Each follows the business context template (see `recipe-06-business-context-doc.md`).

The `context/README.md` is a routing table:

| Trigger | Go to |
|---|---|
| "What is <concept>?" | `<concept>.md` |
| "How do we calculate <metric>?" | `<metric>.md` |

## Step 6 - The workflows/ folder

This holds "how do we do X" docs. One file per workflow. Each follows the workflow template (see `recipe-05-workflow-doc.md`).

The `workflows/README.md` is a routing table from trigger phrases to workflow docs.

## Step 7 - Visualizations placement

Walk me through where visual artifacts go:

- **PDF generators that produce one-off reports** (e.g., a quarterly review PDF) go in `visualizations/`.
- **Interactive dashboards** that are embedded in the platform usually deserve their own top-level `dashboards/` folder, not nested under the team. Reason: dashboard runtimes often expect a specific file layout that's hard to nest.
- **Brand assets** (logos, fonts, color tokens) usually live elsewhere (a separate repo, Drive folder, or design tool). Reference where they live; don't replicate.

## Step 8 - Security and visibility

If this team handles sensitive content, draft the rules now:

- **Compensation / performance / personnel** -> mark explicitly as sensitive in the file header; do not auto-load into general conversations
- **Customer-specific deal data** -> use placeholders (`<Customer A>`) unless the customer has approved being named
- **Internal financials beyond what's already public** -> require explicit ask before surfacing
- **Externally-facing artifacts** -> kept in their own clearly-labeled folder; reviewed before sharing

These rules go at the top of the affected file (a one-paragraph note), not buried at the bottom.

## Step 9 - Personal context placement

Confirm: personal context files for team members do **not** live under the team hub. They live in the top-level `users/` folder. Team membership is captured *inside* each user's `context.md` file, not in the folder path.

If a team member changes roles, their personal context file stays in place.

## Step 10 - Cross-references and nav-table updates

Two updates outside the hub itself:

1. **`shared/ROUTING.md`** (org-wide nav table): if any of this team's content is intended to be reachable by anyone in the org (rare - usually the team's content is gated), add an entry routing the trigger to the team's hub README. Most of the time you will not add anything here; the team's `ROUTING.md` is auto-attached for the role and that is enough.
2. **`<team_name>/ROUTING.md`**: this is the actual file that does the routing for this hub. Make sure every file you created in this session has a row in it pointing from a likely trigger phrase to its path.

## Step 11 - Sense-check

Before producing the patch:

- Does the hub README clearly state the domain boundary?
- Is there a small table mapping the top 10 trigger phrases for this domain to specific files?
- Are sensitive areas clearly marked at the file level?
- Do `context/` and `workflows/` each have a routing README, not just files?
- Have I left personal context out of the team hub?

Then produce the patch.

## Step 12 - Iteration plan

A hub is never "done" in one session. Recommend the order:

1. Hub README and folder skeletons (this session)
2. The 3-5 most-frequently-needed context docs
3. The 3-5 most-frequently-needed workflow docs
4. Visualizations, meetings, and special folders as they accumulate

Set a target: at the end of two weeks of follow-up sessions, the hub should answer the top 20 questions a new team member would ask.
