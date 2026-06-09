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
- Financial services (banking / payments / lending) → **`TextQLLabs/ontology-finserv-starter`**
  (deposits, NIM, delinquency, charge-offs, fraud; MCC / NAICS classification).
- See **`DOMAINS.md`** for the full catalog (verticals + enterprise functions) and per-domain status.
  Each domain starter has its own `SKILL.md` so it's discoverable here.

Run `recipes/recipe-11-start-from-domain-template.md` to do this end to end (connect → validate → adapt).

**2. Net-new domain, building from scratch? Use the recipes.** Drop a recipe into Ana's chat and it
asks 3–7 scoping questions, drafts files, and opens a PR. Start with
`00-START-HERE-ontology-building-guide.md`, then `recipes/recipe-00-scoping-conversation.md`, and
build one slice (20–30 min) at a time.

**3. Starting from existing material?**
- **Have a database** → discovery-first: connect it, let Ana profile the schema and draft the first
  entities/metrics (`recipes/recipe-03-database-or-data-source.md`).
- **Have a pile of documents** (SOPs, metric docs, transcripts, policies) → document-first: Ana reads
  them and proposes the model, narrating where each input lands.

Then, regardless of path: **validate** (`recipes/recipe-12-validate-with-golden-queries.md` — pin
known-correct values, alert on drift), **govern** (`recipes/recipe-13-role-based-access.md` —
per-role surfaces / PHI gating), and **maintain** (every change is a reviewable PR; the ontology
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
- `recipes/` — interactive, drop-into-chat sessions: scoping, nav tables, folder structure, data
  source, API, workflow, business definition, personal context, team area, playbook/dashboard,
  plus the folded-in workshop assets — **start-from-domain-template (11)**,
  **golden-query validation (12)**, **role-based access (13)**.
- `templates/` — skeleton docs the recipes generate from.
- **Domain templates** (separate repos, Path 1) — prebuilt starting points; catalog + status in `DOMAINS.md`.
- **Worked example** — a full mixed-inputs → built-ontology walkthrough lives in the
  ontology-workshop-guide: https://github.com/TextQLLabs/ontology-workshop-guide/tree/main/example-scenario

## When NOT to start from scratch
If a domain template fits (Path 1), use it — it's already built and validated, and you adapt rather
than author. Fall back to the recipes (Path 2/3) only when no template covers the domain.
