# Recipe: Validate Metrics With Golden Queries

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to lock in the correctness of my ontology's metrics with **golden queries** - fixed questions with known-correct answers that I pin and re-check, so I'm alerted when a change drifts a number. Please run this as a guided session - one question at a time.

Golden queries are what turn "the ontology looks right" into "the ontology is provably right and *stays* right." They're the regression test suite for your semantic layer.

## Step 1 - Pick the metrics to pin

Ask which governed metrics matter most - the ones leadership cites, the ones tied to payment or compliance, the ones two teams have argued about. Start with 5-10. Bias toward metrics where a wrong number has real consequences.

## Step 2 - Establish the known-correct value

For each metric, agree on the authoritative answer for a **fixed input** (a specific month, cohort, or period). Sources, best first:
- A number Finance / a regulator already trusts.
- An existing certified report.
- A value computed once, carefully, and reviewed - then treated as the baseline.

If no trusted value exists, that itself is a finding: the metric has never been verified. Compute it deliberately and pin it.

## Step 3 - Write the golden query

For each metric, record the **exact governed call** (which surface, which params) and:
- The **expected value**.
- A **tolerance** - exact for financial / regulatory metrics; a small percentage band for exploratory ones.
- The measurement context (date, cohort, "current date" assumption).

Don't pin a value you got from ad-hoc SQL - pin the value the *governed surface* returns, so the test guards the surface.

## Step 4 - Capture invariants

Beyond exact values, record relationships that must **always** hold regardless of the exact number:
- Rates in `[0, 1]`.
- Superset relationships (all-cause >= condition-specific; any-position >= primary-only).
- Denominator logic (prorated member-months <= full-month count).
- Every externally-reported count >= the suppression threshold.

Invariants catch errors even when the exact value legitimately moves (new month, more data).

## Step 5 - Decide refresh and drift triggers

Ask me:
- When do goldens re-run? (Any ontology PR touching the surface; any schema change; a schedule.)
- What happens on drift? (Block the PR; alert an owner; require a review note that explains the move.)
- Who owns each golden value when it legitimately changes?

## Step 6 - Write it down

Standard structure - one `validation/golden-queries.md`:

```
# Golden queries - pinned values & drift alerts

| # | Surface | Call (params) | Expected | Tolerance | Status |
|---|---------|---------------|----------|-----------|--------|
| 1 | ...     | ...           | ...      | exact/±%  | verified |

## Invariants
- ...

## Refresh / drift policy
- ...
```

Keep a one-line provenance for each expected value (where the known-correct number came from).

## Step 7 - Update the relevant nav table(s)

Add a `trigger -> validation/golden-queries.md` row so "is metric X still correct?" routes here. Org-wide -> `shared/ROUTING.md`; team-scoped -> the team's `ROUTING.md`.

> **Manual UI step**: a new nav table must be bound to its scope in the TextQL app UI before auto-attach works.

## Step 8 - Sense-check

- Does every pinned metric have a tolerance and a provenance line (not an invented number)?
- Do the golden queries call the **governed surface**, not hand-written SQL?
- Are there invariants that hold even as data grows?
- Is the drift policy explicit (who's alerted, what blocks)?

Then produce the patch.
