# Recipe: Document a Business Context Concept (definition, metric, stage, role)

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to write a "what this means" file - a single canonical definition of a term, metric, stage, role, or domain concept that my team uses. Please run this as a guided session.

These are the highest-leverage docs in the ontology. They settle arguments. They prevent Ana from inventing a definition. They become the one place a definition lives.

Note on metrics specifically: if I'm defining a metric that Ana should compute (e.g., Total Revenue, Active Customers), the canonical math also lives in a `.tql` metric declaration on the relevant object. This recipe captures the **business meaning** - the prose, examples, gotchas, and edge cases that don't fit inside `.tql`. The two should reference each other.

## Step 1 - Decide the scope of this definition

**Before doing anything else, decide whose definition this is.** The same concept often means different things to different teams, and that's correct - the ontology supports parallel canonical definitions when they're scoped to the right folder.

Ask me:
- Whose definition is this? (My team only, my function, multiple teams, the whole org?)
- Do other teams already have, or need, their own version of this concept? Common examples:
  - **Revenue** - Sales reads it as closed-won pipeline, Finance as recognized revenue, Product as consumption dollars
  - **Active customer** - Sales might mean "has a closed deal in the last 12 months", Product might mean "logged a qualifying event in the last 30 days", Finance might mean "currently billing"
  - **Churn** - same problem, three different math definitions
  - **Headcount** - HR includes contractors, Finance excludes them, Engineering counts only ICs
- If multiple teams need a version, are we writing one of them today, or all of them?

**Based on my answer, determine the folder placement:**

- **Single team only** -> `<team>/context/<concept>.md` (e.g., `finance/context/revenue.md`). Folder-level RBAC keeps it visible only to that team.
- **Multiple teams, different definitions** -> One file per team, each in its own team folder. Optionally, a `shared/reconciliation/<concept>-bridge.md` that explains how the team definitions relate to each other.
- **Genuinely shared across the whole org** -> `shared/context/<concept>.md`. Use this sparingly. Push back if I'm putting something here that probably has team-specific nuance.

Confirm the scope and the folder path before moving on. This decision drives everything that follows.

## Step 2 - Identify the concept

Ask me:
- The name of the concept
- The 2-3 most common synonyms or misspellings (especially voice-to-text variants)
- Whether this is a metric (a number), a category (a state or stage), a role, or a domain term
- Who owns the definition (which person or team is the authority)
- If it's a metric: does a `.tql` definition already exist, or will I create one as part of this session?

## Step 3 - The one-sentence canonical definition

Force me to write a single sentence. If I can't, the concept is either underdefined or actually two concepts.

Example: "Revenue" might really be three concepts - sales pipeline value, recognized revenue, and cash collected. Surface that ambiguity early and document each separately, with a routing rule that disambiguates.

## Step 4 - What this is NOT

The highest-value section. Ask me what people commonly confuse this with. List each one and explain the distinction.

Examples:
- "Pipeline" is **not** "forecasted revenue" - one is total stage-weighted, the other is filtered for the current quarter
- "Active user" is **not** "logged-in user" - active requires a qualifying event
- "Account Executive" is **not** "sales rep" - sales rep is generic; AE has a specific role in the stage process

## Step 5 - The math (for metrics)

If this is a metric, document the math explicitly:

- The formula in plain English
- The source object(s) and attribute(s) - link to the `.tql` definition
- The exclusions (test data, internal employees, soft-deleted records)
- The time window (calendar day in UTC? business day in your zone? trailing 30?)
- The edge cases (denominator zero, numerator nulls)

If two teams calculate the same metric differently, document both with a routing rule that says when to use which.

The `.md` should link to the `.tql` metric definition, and the `.tql` should link back to this `.md` in a comment. They are the same definition expressed for two audiences.

## Step 6 - The lifecycle (for stages and states)

If this is a state or stage, document:

- All possible values (every stage, in order)
- The transitions (which state can move to which other state)
- The gates or criteria for advancing
- The terminal states (which states are "done")
- Probability or weighting (if relevant for forecasting)

For each state, a sub-section with:
- Plain definition
- Required milestones to enter
- Required milestones to advance out
- Owner (which person or team is responsible)

## Step 7 - The role (for roles)

If this is a role, document:

- One-sentence definition
- Responsibilities owned
- Handoffs (who hands to this role, who this role hands to)
- Decisions this role makes that no one else can
- Systems or tools this role uses

## Step 8 - Examples and counterexamples

For any concept type, include 2-3 examples and 1-2 counterexamples. Real examples cement the abstract definition.

> Example: "<Customer A> in <Stage X> with a target close date of <Q3> counts toward Q3 pipeline."
>
> Counterexample: "<Customer B> moved from Q3 to Q4 close last week. They no longer count toward Q3 pipeline."

If actual customer / deal names are sensitive, use placeholders.

## Step 9 - Cross-references

List every other file that touches this concept:

- Any `.tql` that uses the metric
- Any workflow that depends on this stage
- Any other `.md` that paraphrases this definition - update each to link here instead of redefining

This is the "one source of truth" rule in action.

## Step 10 - Draft and review

Standard structure:

```
# <Concept Name>

> One-sentence canonical definition.

## What this is NOT
- ...

## The math / lifecycle / responsibilities
...

## Examples
- ...

## Counterexamples
- ...

## Edge cases and gotchas
- ...

## Sources
Where this definition came from (team offsite, owner email, etc.)

## Cross-references
- Linked .tql: `<path/to/object.tql>` (the metric declaration)
- Linked workflows: ...
- Linked dashboards: ...
```

Show me the draft. Walk through. Let me edit.

## Step 11 - Folder placement

Business context lives in a `context/` folder under the relevant team or functional area:

- `go_to_market/context/deal-stages.md`
- `finance/context/revenue-recognition.md`
- `engineering/context/incident-severity.md`

If this is the first context file for a team, also create the `context/README.md` as an index.

## Step 12 - Update the relevant nav table(s)

Add or update entries in the relevant nav table(s) so this slice becomes discoverable:

- If this slice is **org-wide** (every user should be able to reach it), add a row in `shared/ROUTING.md`.
- If this slice is **team-scoped** (only one team's users should reach it), add a row in that team's `<team>/ROUTING.md`.
- If this slice is **connector-scoped** (only loads when the connector is active), add it to the connector's nav table.
- If a target nav table does not exist yet, this is a signal to run `recipe-01-nav-tables.md` after this one.

The added rows should be `trigger phrase -> path/to/file.md` entries, not prose definitions.

> **Manual UI step**: If you create a new nav table here, remember it does not auto-attach automatically. An admin has to bind it to the right scope (org, role, connector) in the TextQL app UI before the auto-attach takes effect.

## Step 13 - Sense-check

Before producing the patch:

- Is the canonical definition exactly one sentence?
- Is there a "what this is NOT" section?
- Is the math / lifecycle / responsibilities concrete enough that two analysts would produce the same number / outcome?
- Are examples specific (with placeholders for sensitive details)?
- For a metric: does the `.tql` metric definition match this prose, and do they link to each other?
- Have I checked whether other docs paraphrase this concept and need to be updated to link here?

Then produce the patch.
