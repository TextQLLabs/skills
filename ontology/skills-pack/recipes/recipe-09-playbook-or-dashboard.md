# Recipe: Document a Playbook or Dashboard

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to document a scheduled artifact: either a **playbook** (an automated analysis that runs on a schedule and posts results) or a **dashboard** (an interactive visualization embedded in the platform). Please run this as a guided session.

## Step 1 - Which one am I documenting?

Ask me which:

- **Playbook**: runs on a cron schedule, produces a report, posts to a destination (Slack, email)
- **Dashboard**: rendered in the platform, queried interactively, no schedule

The structure differs, so confirm before proceeding.

---

## If documenting a PLAYBOOK

### Step 2P - Identify the playbook

Ask me:
- The name of the playbook
- The schedule (cron expression or human description)
- The destination (Slack channel, email list, both)
- The recipients
- The owner (which person or team is responsible)
- Whether it's templatized (parameters passed in via the playbook prompt) or one-off

### Step 3P - The mission statement

Every playbook starts with a `playbook-mission.md` that answers:

1. **Purpose**: what business question this answers
2. **Audience**: who reads it and what decision it informs
3. **Schedule**: when it runs and why that cadence
4. **Inputs**: the data sources it pulls from
5. **Outputs**: what the report looks like (sections, charts, narrative)
6. **Methodology**: the calculations and any non-obvious choices
7. **Edge cases**: weekends, holidays, missing data

Draft this with me, section by section.

### Step 4P - The code structure

If the playbook involves code (queries, transformations, visualizations), recommend this layout:

```
playbooks/<playbook-name>/
  playbook-mission.md      <- the mission statement
  REFERENCES.md            <- cross-reference to underlying queries/APIs
  queries.py               <- (optional) extracted SQL/data fetching
  analysis.py              <- (optional) transformations and calculations
  plotting.py              <- (optional) chart generation
  report_generation.py     <- (optional) report assembly
```

For simpler playbooks, a single script is fine. Don't over-structure.

### Step 5P - REFERENCES.md

This is a small file that points to every external doc the playbook depends on. Standard format:

```
| Dependency Type | Reference |
|---|---|
| Database | <path to database README> |
| Metric definition | <path to canonical metric doc> |
| Shared helper | <path to shared module> |
| Slack channel | <channel name + ID if known> |
```

This is what protects against silent breakage when underlying schemas change.

### Step 6P - Templatization

If the playbook is templatized (one script, many instances differing only in parameters):

- Document the parameter contract (what variables the prompt passes in, and their valid values)
- Show one example of an instantiated prompt
- Note any variables that have safe defaults vs that must be supplied

### Step 7P - Update the relevant nav table(s)

Add or update entries in the relevant nav table(s) so this slice becomes discoverable:

- If this slice is **org-wide** (every user should be able to reach it), add a row in `shared/ROUTING.md`.
- If this slice is **team-scoped** (only one team's users should reach it), add a row in that team's `<team>/ROUTING.md`.
- If this slice is **connector-scoped** (only loads when the connector is active), add it to the connector's nav table.
- If a target nav table does not exist yet, this is a signal to run `recipe-01-nav-tables.md` after this one.

The added rows should be `trigger phrase -> path/to/file.md` entries, not prose definitions.

> **Manual UI step**: If you create a new nav table here, remember it does not auto-attach automatically. An admin has to bind it to the right scope (org, role, connector) in the TextQL app UI before the auto-attach takes effect.

For playbooks specifically, add a row like `"<playbook trigger phrase>" -> <link to mission doc or its Slack channel>` so a user asking about that recurring report knows which playbook owns it.

---

## If documenting a DASHBOARD

### Step 2D - Identify the dashboard

Ask me:
- The name of the dashboard
- The domain (sales, finance, engineering, etc.)
- Who uses it and how often
- The underlying data sources
- Whether it's standalone or part of a multi-tab suite
- Any non-obvious metric definitions or filters baked in

### Step 3D - The README

Every dashboard folder gets a `README.md` answering:

1. **Purpose**: what this dashboard shows
2. **Audience**: who uses it
3. **Data sources**: where the numbers come from
4. **How to read it**: any non-obvious conventions (timezone, what "active" means, etc.)
5. **How to run it locally** (if relevant)
6. **How the runtime loads it** (if the platform's dashboard runtime needs a specific layout)
7. **Known gotchas**: things users frequently misread

### Step 4D - The code structure

Typical layout:

```
dashboards/<dashboard-name>/
  README.md
  app.py                   <- entry point (or main.py)
  config.py                <- constants, color palette
  data_pipeline.py         <- data fetching
  helpers.py               <- shared utilities
  styles.py                <- visual styling
  charts/                  <- one file per chart
    <chart_1>.py
    <chart_2>.py
  components/              <- reusable UI primitives
    <component_1>.py
  sections/                <- per-tab logic if multi-tab
    <section_1>.py
```

Recommend collapsing to a single file if the dashboard is small.

### Step 5D - Data sources doc

If the dashboard pulls from multiple sources or has non-trivial joins, add a `data_sources.md` (or `architecture.md`) that explains:

- Which databases are queried
- The join logic
- Any caching or pre-computation
- Refresh cadence

### Step 6D - Update the relevant nav table(s)

Add the dashboard to the top-level `dashboards/` index, and also:

Add or update entries in the relevant nav table(s) so this slice becomes discoverable:

- If this slice is **org-wide** (every user should be able to reach it), add a row in `shared/ROUTING.md`.
- If this slice is **team-scoped** (only one team's users should reach it), add a row in that team's `<team>/ROUTING.md`.
- If this slice is **connector-scoped** (only loads when the connector is active), add it to the connector's nav table.
- If a target nav table does not exist yet, this is a signal to run `recipe-01-nav-tables.md` after this one.

The added rows should be `trigger phrase -> path/to/file.md` entries, not prose definitions.

> **Manual UI step**: If you create a new nav table here, remember it does not auto-attach automatically. An admin has to bind it to the right scope (org, role, connector) in the TextQL app UI before the auto-attach takes effect.

For dashboards specifically, add a row like `"<dashboard trigger phrase>" -> <path to dashboard README>` so a user asking about that area of the business knows which dashboard owns the canonical view.

---

## Step 8 - Cross-references (both)

Identify every:
- Database / API the artifact uses (link to its README)
- Business context concept it relies on (link to the canonical definition)
- Workflow that depends on its output
- Personal context file that names it

Update each of those to back-link if appropriate.

## Step 9 - Sense-check (both)

Before producing the patch:

- Is the purpose stated in one sentence?
- Are the data sources explicit and linked?
- Are the metric definitions in this artifact aligned with the canonical definitions elsewhere in the library? (If not, fix the artifact or fix the canonical doc - never let them drift)
- Is the README readable by someone who has never seen the artifact before?

Then produce the patch.
