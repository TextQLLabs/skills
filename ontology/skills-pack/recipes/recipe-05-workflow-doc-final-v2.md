# Recipe: Document a Workflow

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to document one repeated workflow - a multi-step process my team executes the same way every time. Please run this as a guided session.

## Step 1 - Identify the workflow

Ask me:
- The trigger - what causes this workflow to start? (a phrase, an event, a schedule)
- The desired end-state - what does "done" look like?
- Roughly how often it runs (daily, weekly, on-demand)
- How long it currently takes a human
- Whether the workflow exists in someone's head, in a Notion doc, or scattered across Slack threads

## Step 2 - List every trigger phrase

Ask me to enumerate every way someone might ask for this workflow. Examples for a CRM "update deal" workflow:

- "Update the deal"
- "Update the deal for <Company>"
- "Add call notes for <Company>"
- "We just had a call with <Company>"
- "Here's the recording from <Company>"

Or for a finance "monthly close" workflow:

- "Run monthly close"
- "Close the books"
- "Generate the month-end pack"

These trigger phrases go at the top of the workflow doc and into the relevant nav table(s) (see Step 9).

## Step 3 - Walk through the steps

This is the bulk of the session. Walk me through the workflow slowly. For each step:

1. **Plain English description** of what happens
2. **Which system is touched** (CRM, warehouse, Slack, email, spreadsheet, file storage)
3. **What inputs the step needs** (data from a previous step, a user-supplied parameter)
4. **What output the step produces** (a record updated, a file generated, a message sent)
5. **Any conditional logic** ("if the company doesn't exist yet, create it; otherwise update it")

A common mistake: humans skip steps because they're "obvious." Push back. "What if the company doesn't exist in the CRM yet?" "What if there's already a record from yesterday?" "What if the recording is missing?"

## Step 4 - Identify the unprompted-vs-prompted gotchas

For each step, ask: should Ana do this automatically when triggered, or should it ask first?

Workflows often fail because Ana asks "should I also update the CRM?" when the user clearly said "update the deal" - which means update everything. Document the **full checklist Ana should execute without being asked** vs the **explicit confirmations Ana must seek**.

## Step 5 - Sense-check steps and guardrails

For each step, ask: what could go wrong?

- Are there closed records that should never be modified?
- Are there destructive operations that need explicit confirmation?
- Is there a "this is irreversible, double-check first" step?
- Are there steps that should not happen on weekends, holidays, or outside business hours?

These belong in a dedicated "Guardrails" section at the bottom of the doc.

## Step 6 - Edge cases

Capture the 3-5 most common deviations from the happy path:

- "What if the input is missing?"
- "What if the record already exists?"
- "What if a downstream step fails?"
- "What if the user provides ambiguous input (e.g., two companies with similar names)?"

For each, document the right behavior. Vague "Ana will figure it out" is not acceptable here.

## Step 7 - Draft the doc

Standard structure:

```
# Workflow: <Name>

## Purpose
One paragraph: what this does and why.

## When to use
Bullet list of trigger phrases.

## What this is NOT
Bullet list of common misunderstandings or wrong applications.

## The full checklist (execute without being asked)
Step 1: ...
Step 2: ...
...

## Edge cases
- What if X is missing -> do Y
- What if Z already exists -> do W

## Guardrails
- Never do X to a closed record
- Always confirm Y before doing Z

## Cross-references
- Related workflows: ...
- Underlying APIs / databases: ...
- Business context: ...
```

Show me the draft. Walk through each section. Let me push back.

## Step 8 - Folder placement

Workflows live with the team or functional area they serve. Suggest a path like:

```
<functional_area>/workflows/<workflow-name>.md
```

For example: `go_to_market/workflows/update-deal.md`, `finance/workflows/monthly-close.md`.

Create the folder's README index entry if it doesn't exist yet.

## Step 9 - Update the relevant nav table(s)

Add or update entries in the relevant nav table(s) so this slice becomes discoverable:

- If this slice is **org-wide** (every user should be able to reach it), add a row in `shared/ROUTING.md`.
- If this slice is **team-scoped** (only one team's users should reach it), add a row in that team's `<team>/ROUTING.md`.
- If this slice is **connector-scoped** (only loads when the connector is active), add it to the connector's nav table.
- If a target nav table does not exist yet, this is a signal to run `recipe-01-nav-tables-final-v2.md` after this one.

The added rows should be `trigger phrase -> path/to/file.md` entries, not prose definitions.

> **Manual UI step**: If you create a new nav table here, remember it does not auto-attach automatically. An admin has to bind it to the right scope (org, role, connector) in the TextQL app UI before the auto-attach takes effect.

## Step 10 - Sense-check

Before producing the patch:

- Are the trigger phrases comprehensive (5+ ways someone might ask)?
- Does the checklist cover every step, including the ones a human would skip as "obvious"?
- Are guardrails concrete and testable?
- Are edge cases enumerated, not left as "Ana will handle it"?
- Have I linked to underlying APIs / databases / business context?

Then produce the patch.
