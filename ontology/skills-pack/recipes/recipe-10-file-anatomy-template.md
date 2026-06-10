# Recipe: File Anatomy Template (reference)

> **How to use this file**: This recipe isn't a guided session - it's a *reference* for the inner structure of any file in the ontology. Drop it into chat any time you want Ana to review a draft against the canonical shape, or use it as a checklist when authoring by hand.

---

The ontology is multi-format. The exact shape of a file depends on its type. Most of this reference describes Markdown (`.md`) docs because those are where unstructured context lives. There's also a short section at the end on `.tql` conventions.

## File types you'll see in an ontology

| Extension | Purpose | Typical home |
|---|---|---|
| `.tql` | Semantic model: objects, attributes, links, metrics, composed query views | `objects/`, `queries/` |
| `.md` | Unstructured context: definitions, workflows, READMEs, gotchas, meeting notes | Almost everywhere |
| `.py` | Helper scripts, dashboard code, custom metric implementations | `playbooks/`, `dashboards/`, `skills/` |
| `.csv` / `.tsv` / `.parquet` | Static reference data, exports, snapshots | `datasets/` |
| `.xlsx` | Spreadsheet inputs/outputs (forecasts, templates) | Co-located with the workflow that uses them |
| `.html` | Embeddable reports, dashboard fragments | `dashboards/`, `playbooks/` |
| `.png` / `.jpg` / `.svg` | Brand assets, diagrams, screenshots | A brand or assets folder |
| `.pptx` / `.pdf` | Slide templates, signed PDFs, distributed reports | Templates folder or workflow folder |
| `.yaml` / `.json` | Configuration (targets, quotas, mappings) | Co-located with the thing it configures |

The ontology stores anything Ana might need to read or produce. Treat the file type as a contract about how it'll be used, not a barrier to inclusion.

## The universal sections (for `.md` files)

In order from top to bottom:

### 1. Title

A single H1. Names the thing. Examples:

- `# Update Deal Workflow`
- `# Table: acu_usage`
- `# Deal Stages`

Boring is correct.

### 2. One-line purpose

Immediately under the title. One sentence.

> Records every ACU consumption event with org, member, and category attribution.

If you can't write this in one sentence, the doc's scope is too broad. Split it.

### 3. Front-matter pointers (when relevant)

For some doc types, a few key facts go right at the top in a blockquote:

```
> Email: <user>@<org>.com
> Role: <role>
> Last updated: <date>
```

For tables:

```
> Database: <name>
> Backing object: <path/to/object.tql>
> Access: read-only
```

For APIs:

```
> Base URL: <url>
> Auth: <method>
> Rate limit: <limit>
```

### 4. Cross-reference block (often a blockquote callout)

The most important *related* files. Examples:

> For X-related writes, use [<other doc>](path). This doc covers reads.

> The metric math lives in [`<object>.tql`](path). This doc covers the business meaning.

This is where you implement the "one source of truth" rule - if a concept defined elsewhere shows up here, link it. Do not redefine.

### 5. Body sections

H2s for top-level, H3s for sub-sections. Rarely go deeper than H3.

Typical bodies by doc type:

#### Database README
- How to query this database
- Objects of interest (object -> `.tql`)
- Queries of interest (query -> what it answers)
- Cross-DB relationships

#### Table / object context doc
- Schema (column table)
- Backing object link (the `.tql`)
- Critical gotchas
- Timezone / convention
- Common joins
- Cross-references

#### API README
- Auth snippet
- Rate limit / base URL
- READ vs WRITE routing rule
- Pre-write safety link
- Where each operation lives
- Key endpoints

#### Workflow doc
- When to use (trigger phrases)
- What this is NOT
- Full checklist (numbered steps)
- Edge cases
- Guardrails

#### Business context doc (definition / metric / stage / role)
- One-sentence definition
- What this is NOT
- Math / lifecycle / responsibilities
- Linked `.tql` (for metrics)
- Examples and counterexamples
- Edge cases

#### Personal context doc
- Communication style
- Common misspellings
- Names I use for things
- My workflows
- Notification preferences

### 6. Sense-check / guardrails (when relevant)

For workflow and write-API docs, a guardrails section near the end. Concrete, testable rules. "Never do X to Y" not "be careful."

### 7. Cross-references at the bottom

A "See also" section. The 3-5 most relevant adjacent files. Each as a bullet with a one-line description.

### 8. (Optional) Sources / changelog

For docs that capture a moment in time (a stage definition decided at an offsite, a metric pinned by Finance), note the source.

```
Source: <Team> offsite, <date>
Last updated: <date>
```

---

## `.tql` file conventions

> **Building a `.tql` file from scratch?** Use `recipe-11-tql-authoring.md` (guided session — Ana picks the shape for you) and `templates/tql-file.md` (copy-pasteable skeletons for the three most common shapes). This section covers review conventions; those two files cover authoring.

`.tql` files are the semantic model. Their shape is enforced by the language, but a few conventions make them easier for both humans and Ana to read:

- **Top-of-file comments** describe the object or composed view in plain English. Include the fact and dimensions.
- **Imports** at the top of the file. One import per line.
- **`params` block** declares what the file accepts (metrics, dimensions, filters).
- **`let` bindings** for intermediate values - the metric fragments, dimension entries, join helpers.
- **`in sql''` block** at the bottom assembles the final SQL.

A well-formed `.tql` file is small and reviewable. If a file exceeds ~150 lines, consider splitting:
- Object definitions belong in `objects/<name>.tql`
- Composed query views belong in `queries/<name>.tql`
- Shared helpers belong in a `_lib/` folder

For metric declarations specifically: every metric should have a `.md` counterpart in `context/` that explains the business meaning, the exclusions, and the time grain. The two reference each other.

---

## The universal rules

### Rule 1: Routing tables over prose

When a doc lists more than 3 of something with associated paths, make it a table:

| Item | Where to go |
|---|---|
| ... | ... |

Tables are scannable. Prose is not.

### Rule 2: Code blocks are runnable

Any code block (SQL, Python, shell) should be runnable as-is after substituting placeholders. Mark placeholders clearly: `<your_org_id>`, `<database_name>`.

### Rule 3: Links are paths, not English

Write `[doc](path/to/doc.md)`, not "see the document in the workflows folder."

### Rule 4: No dates in file names

Dates inside files, not in file names. Exception: meeting notes (`2026-02-18-offsite.md`) where the date *is* the identity.

### Rule 5: Front-load the answer

If 80% of readers come to a doc for one fact, that fact goes near the top.

### Rule 6: Be specific, not aspirational

"Ana should always update the deal" is aspirational. "When the user pastes a Grain URL, Ana extracts the transcript, finds the deal, updates Notion call notes, updates Attio milestones, and updates contacts" is specific.

---

## Use this as a review checklist

When reviewing a draft, run through:

- [ ] Is the title boring and clear?
- [ ] Is there a one-sentence purpose right under the title?
- [ ] Is the most important cross-reference within the first 10 lines?
- [ ] Are lists of 4+ things rendered as tables?
- [ ] Are all code blocks runnable as-is?
- [ ] Are all internal links real paths (not English descriptions)?
- [ ] Is the answer to the most common question front-loaded?
- [ ] Are guardrails / gotchas concrete?
- [ ] Is the doc readable to someone who has never seen the system before?
- [ ] Have I avoided redefining anything that's defined elsewhere?
- [ ] For metrics: does the `.md` link to the `.tql` and vice versa?

If yes across the board, the doc is in good shape.

---

## When to drop this recipe into a chat

- "I'm about to write a workflow doc; remind me what sections it should have"
- "Review this draft against the canonical shape and tell me what's missing"
- "I'm onboarding a new team member - what does a good ontology file look like?"

This recipe is the only one in the kit that doesn't trigger a guided session. It's pure reference.
