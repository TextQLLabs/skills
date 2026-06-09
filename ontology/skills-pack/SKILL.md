---
name: ontology-builder
description: Build, extend, or stand up a governed TextQL ontology (the git-backed data context Ana reads as ground truth) — from a prebuilt domain template, from scratch with interactive recipes, or from an existing database or pile of documents. Use when a user wants to create, grow, or govern an ontology / semantic layer for their data in TextQL/Ana.
---

# Ontology Builder

Build a **governed ontology** — the versioned, git-backed definition of how an organization's data
maps to its business (entities, metrics, rules) that Ana reads as ground truth, so questions get
answered with consistent, reviewable SQL instead of guesswork.

## Choose your path (route here first)

**1. In a covered domain (healthcare / life-sciences, financial services, …)? Start from a prebuilt
domain template — don't build blank.** A template ships entities, governed metric surfaces,
terminology/code-set crosswalks, governance, and validated golden queries; you connect it, point it
at the customer's data, and adapt. Hours, not weeks — and already validated.
- Healthcare & life sciences → **`TextQLLabs/ontology-healthcare-starter`** (claims/clinical:
  PMPM, readmission, prevalence, HCC/RAF, HEDIS; ICD-10 / CCSR / CMS-HCC terminology).
- More domains coming. (Each domain starter has its own `SKILL.md` so it's discoverable here.)

**2. Net-new domain, building from scratch? Use the recipes.** Drop a recipe into Ana's chat and it
asks 3–7 scoping questions, drafts files, and opens a PR. Start with
`00-START-HERE-ontology-building-guide.md`, then `recipes/recipe-00-scoping-conversation.md`, and
build one slice (20–30 min) at a time.

**3. Starting from existing material?**
- **Have a database** → discovery-first: connect it, let Ana profile the schema and draft the first
  entities/metrics (`recipes/recipe-03-database-or-data-source.md`).
- **Have a pile of documents** (SOPs, metric docs, transcripts, policies) → document-first: Ana reads
  them and proposes the model, narrating where each input lands.

Then, regardless of path: **validate** (pin golden-query values, alert on drift), **govern**
(role-based access / PHI gating), and **maintain** (every change is a reviewable PR; the ontology
grows continuously — you never start over).

## Core principles (every path)
- **One source of truth per scope** — each concept has exactly one canonical definition in its
  scope; everything else points to it. RBAC gates which team sees which version.
- **Routing tables over prose** — a small `trigger → file` table is the entry point and the only
  auto-attached file; prose lives in the linked files, pulled in only when relevant.
- **Governed surfaces, not ad-hoc SQL** — metrics are `.tql` that render inspectable warehouse SQL;
  changes are proposed → diffed → reviewed → approved → audited, like code.
- **Discovery first** — build from what's actually in the schema/documents, not assumptions.

## What's in this pack
- `00-START-HERE-ontology-building-guide.md` — the from-scratch method and the three rules above.
- `recipes/` — 10 interactive, drop-into-chat sessions (data source, API, workflow, business
  definition, personal context, team area, playbook/dashboard, nav tables, folder structure).
- `templates/` — skeleton docs the recipes generate from.
- **Domain templates** (separate repos, Path 1) — prebuilt, validated starting points.

## When NOT to start from scratch
If a domain template fits (Path 1), use it — it's already built and validated, and you adapt rather
than author. Fall back to the recipes (Path 2/3) only when no template covers the domain.
