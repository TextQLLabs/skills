# Recipe: Scoping Conversation (start here if you're new)

> **How to use this file**: Copy everything below the `---` and paste it as your first message in a new chat. Ana will guide the rest.

---

I want to start building (or extending) my organization's ontology with you. The ontology is Ana's unified, living memory of my business - one git-backed repository holding every definition, metric, script, document, dataset, dashboard, and asset Ana needs to reason about my data. It mixes `.tql` semantic models, `.md` context and workflows, `.py` scripts, `.csv` and `.xlsx` data, `.html` and `.png` and `.pptx` visuals, and configuration files - everything Ana can use.

Before we write anything, help me scope today's session. Walk me through this step by step, asking one focused question at a time and waiting for my answer. Do not generate any files yet.

**Step 1 - Inventory.** Ask me what I currently have:
- Which data sources my team uses (databases, warehouses, APIs)
- Whether we already have documentation anywhere (Notion, Confluence, Google Docs, scattered Markdown)
- Which metrics and definitions my team relies on day-to-day
- Which workflows are repeated weekly or daily
- Whether we have an existing ontology or this is greenfield

**Step 2 - Pain points.** Ask what's currently broken:
- What questions does my team have to ask the same person over and over
- Which metric definitions does my team argue about
- What does Ana (or any other tool) currently get wrong because it lacks context
- What manual rituals would I love Ana to automate

**Step 3 - Audience and access.** Ask who will use this:
- Just me, or a team?
- One functional area (engineering, sales, finance) or cross-functional?
- Are there sensitive areas (comp, M&A, board materials) that need access boundaries?
- Will any of this need to be shared externally (with a customer, partner, auditor)?

**Step 4 - Scope for today.** Based on my answers, recommend a single slice. Bias toward the smallest possible win - a 20-minute session that produces one well-formed file is better than two hours producing overwhelming sprawl. Options:

- One database modeled as ontology objects with metrics and canonical queries
- One API integration documented
- One end-to-end workflow
- The canonical definition of one term, metric, or business concept
- My personal context profile (if I'm the first user)
- The org-wide and/or role-scoped nav tables (only if I already have a few documented slices to route to)

**Step 5 - Recipe selection.** Once we agree on the slice, tell me which recipe to drop in next:

| Recipe | When to use |
|---|---|
| `recipe-01-nav-tables-final-v2.md` | After 3-4 slices exist; builds the master nav |
| `recipe-02-folder-structure-final-v2.md` | When flat layout is getting messy and we need to decide on folders |
| `recipe-03-database-or-data-source-final-v2.md` | Modeling a database into objects / metrics / .tql queries |
| `recipe-04-api-integration-final-v2.md` | Documenting an external API integration |
| `recipe-05-workflow-doc-final-v2.md` | A repeated multi-step process |
| `recipe-06-business-context-doc-final-v2.md` | Defining a term, stage, role, or domain concept |
| `recipe-07-personal-context-final-v2.md` | Building or updating a per-user preference file |
| `recipe-08-team-or-functional-area-final-v2.md` | The hub for a whole team's content |
| `recipe-09-playbook-or-dashboard-final-v2.md` | A scheduled report or dashboard |
| `recipe-10-file-anatomy-template-final-v2.md` | Reference for the inner structure of any doc |

**Step 6 - Set expectations.** Before we end this conversation, summarize:
- The slice we picked for today
- The recipe I should drop in next
- Roughly how long it will take (10-30 min)
- What I need on hand (a sample query, list of tables, transcript of the workflow, etc.)

Do not start writing real files in this scoping conversation. The goal here is only to plan. The real work happens in the next chat when I drop in the next recipe.
