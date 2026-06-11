# Recipe: Govern an Ontology With Role-Based Access & Personas

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

One governed ontology can serve many teams - clinical, operations, finance, compliance - each needing its own data scope and response style, **without changing the underlying metric definitions**. I want to define those roles and how Ana behaves for each. Please run this as a guided session - one question at a time.

The governed `.tql` metrics stay identical for everyone. What changes per role is: which surfaces are reachable, how Ana clarifies ambiguity, how it responds, and what it must refuse. Swapping the behavior context swaps the persona; the math never forks.

## Step 1 - Enumerate the roles

Ask which roles / personas use the ontology and, in one line each, what they're responsible for. Typical splits: executive, analyst, operations, clinical, finance, compliance. Don't over-segment - start with the 3-5 that actually have different needs.

## Step 2 - Allowed data surfaces (per role)

For each role, ask:
- Which query surfaces / folders may it reach?
- Which areas are **off-limits** (PHI / member-level detail, compensation, raw financials, sensitive diagnoses)?

Map allowances to **folders** so RBAC gates by path. A surface a role can't reach shouldn't even be discoverable to it.

## Step 3 - Clarification behavior

How should Ana handle an ambiguous request for this role?
- **Restricted / business user** -> menu-driven: offer the governed options and let them pick; don't free-hand.
- **Analyst** -> assumption-transparent: state the assumption, proceed, show the work.

## Step 4 - Response style

Per role: SQL visibility (show it / hide it), tone, detail level, output format.
- An **executive** wants the number and one line of context.
- An **analyst** wants the SQL and the caveats.
- An **operations** user wants the row-level worklist (within their allowed scope).

## Step 5 - Hard limits (fail-closed)

What must Ana **decline** for this role? Examples:
- Member-level PHI for anyone without explicit authorization.
- Anything that would re-identify a suppressed small cell.
- Cross-team data a role isn't scoped to (e.g. comp for a non-HR role).

State these as refusals, not soft preferences. When in doubt, decline.

## Step 6 - Write the behavior context files

One `org_context.md` per role, in a `behaviors/` folder. Each has the same four sections:

```
behaviors/
  <role>/org_context.md
```
1. **Allowed data surfaces** - which folders / surfaces this role may query.
2. **Clarification behavior** - menu-driven vs. assumption-transparent.
3. **Response style** - SQL visibility, tone, detail, format.
4. **Hard limits** - what Ana must refuse.

Swapping which `org_context.md` is active swaps the persona; the governed `.tql` is untouched. Show me the draft for the first role, get approval on shape, then produce the rest.

## Step 7 - Update the relevant nav table(s)

Add `trigger -> behaviors/<role>/org_context.md` rows. These are usually role-scoped, so they belong in the role's / team's `ROUTING.md`, not the org-wide one.

> **Manual UI step**: behavior contexts and role-scoped nav tables must be **bound to the role** in the TextQL app UI - this is where RBAC is actually enforced. The files express intent; the app binds it.

## Step 8 - Sense-check

- Does each role have all four sections filled (surfaces, clarification, style, hard limits)?
- Do the governed metric definitions stay identical across roles (only access/behavior differ)?
- Are the hard limits written as fail-closed refusals?
- Is every sensitive surface unreachable - not just unlisted - for roles that shouldn't see it?
- Have the role bindings been set in the app UI (not just the files)?

Then produce the patch.
