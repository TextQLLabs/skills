# Recipe: Document a Database / Define Its Objects, Metrics, and Queries

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to bring one database (or data warehouse / data source) into the ontology so Ana can reason about it correctly. A complete data source slice mixes several kinds of files:

- **`.tql` semantic models** for the objects, attributes, links, and metrics in `.tql` - so Ana queries the warehouse in business terms, not raw table names
- **`.tql` composed query views** for the canonical questions my team asks every week
- **`.md` context files** for the gotchas, conventions, timezone rules, exclusions, and tribal knowledge that a schema alone cannot convey
- **`.md` README** at the source root that routes questions to the right object or query
- Optionally **`.csv`/`.xlsx` reference data**, **`.py` helpers**, or **diagrams** that support the queries

Modeling in `.tql` *and* writing the unstructured context together is what makes the difference between Ana running syntactically correct SQL and Ana running semantically correct SQL.

Please run this as a guided session - ask one question at a time and wait for my answer before producing files.

## Step 1 - Identify the data source

Ask me:
- The name people on my team use for it
- The type (Postgres, Snowflake, BigQuery, Redshift, MySQL, etc.)
- The access mode (read-only / read-write)
- The high-level purpose ("our product DB", "the warehouse", "the billing system")
- The domain it covers (sales, finance, product usage, infrastructure cost)

If my organization has multiple sources that overlap in concept (e.g., a product DB and a billing DB that both contain user info), ask me to clarify which questions belong to which.

## Step 2 - The "multiple sources, one question" framing

Many teams have multiple sources that sound like they could answer the same question. Surface this early:

> Example: "How much money?" can mean
> 1. **Sales** (pipeline forecast from the CRM)
> 2. **Product Usage** (consumption revenue from a metering DB)
> 3. **Billing** (invoices and payments from Stripe)
>
> These are three different numbers from three different sources. The ontology has to route the question to the right one.

If this applies, draft a routing rule (a small table or paragraph) that resolves it.

## Step 3 - Inventory the objects we'll need

In ontology terms, every documented entity becomes an **object** in `.tql`. Each object maps to a table or a SQL query.

Ask me to list the 5-15 entities my team actually queries. For each:

- Business name (Customer, Order, Subscription, Deal)
- Backing table or SQL query
- One-sentence description of what each row represents
- Approximate grain (one row per X)
- Whether it's "current" (mutates in place) or "changelog" (append-only)

Bias toward fewer, well-defined objects. If 100+ tables exist, only ~10-30 actually deserve to be modeled as objects. The rest are looked up live as needed.

## Step 4 - Identify attributes (primary key / dimension / measure)

For each object, walk through its key columns and classify each one:

- **Primary key (PK)** - unique identifier. Exactly one per object. Optional but useful as the default join key.
- **Dimension (dim)** - text, date, or category used to group / filter / slice. "If you'd group by it, it's a dimension."
- **Measure (meas)** - numeric column you'd sum, average, or count.

I don't need to classify every column - just the ones I'll actually query against. Attributes can be added later.

## Step 5 - Identify links between objects

Ask me how the objects connect. For each relationship:

- Source object and destination object
- Cardinality (one-to-one, one-to-many, many-to-one, many-to-many)
- Join keys (which attribute on each side - PK on one side is the default)
- A custom join formula if the join isn't a simple equality

If the same join recurs in many queries, this is exactly the kind of repetition links eliminate.

## Step 6 - Define the metrics

Ask me: what are the 5-10 numbers my team calculates repeatedly?

For each one:
- Business name (Total Revenue, Active Customers, Average Order Value)
- The aggregation (SUM, COUNT, AVG, custom formula)
- The source attribute(s) or SQL expression
- The required filters (exclude test data, exclude internal employees, exclude soft-deletes)
- The time grain (per day, per month, rolling 30)
- The owner - which person or team is the authority for this definition

These become metric definitions in the relevant object's `.tql` file. They are the single highest-leverage thing in the entire ontology - one canonical definition that everyone uses.

## Step 7 - Surface the gotchas (the unstructured context)

For each object, ask:
- What is the most common mistake people make querying it?
- Are there rows that look like data but should be excluded (test data, internal employees, deleted-but-not-gone)?
- What is the timezone convention - UTC, local?
- Are there columns whose names mean something non-obvious?
- Are there foreign keys to other objects that aren't in the schema?

These gotchas don't live in `.tql` - they live in `.md` files alongside the `.tql`, where Ana can pull them in by reference. Without them Ana will get the SQL syntactically right and semantically wrong.

## Step 8 - Capture canonical queries

Ask me: what are 5-10 questions my team asks of this database every week?

For each:
- Write the question in plain English
- Write the canonical answer as a `.tql` query (parameterized where useful)
- Note the parameters that should vary (date range, customer ID, etc.)

`.tql` files compile to native warehouse SQL. They're how you stop Ana from writing fresh SQL and getting it wrong. Every time a question matches a canonical `.tql`, Ana should use it.

> **New to `.tql`?** Drop `recipe-11-tql-authoring.md` into a new chat — it runs a guided session for each file and picks the right shape for you. `templates/tql-file.md` has copy-pasteable skeletons for the three most common shapes (plain query, composed view, `_defs` fragment).

## Step 9 - Folder layout

Recommend this structure (adapt to my conventions):

```
<source_name>/
  README.md                    <- overview, connection info, routing table
  relationships.md             <- how this source joins to other sources (if relevant)
  objects/
    <object_1>.tql             <- object definition (backing, attributes, metrics)
    <object_2>.tql
    ...
  queries/
    README.md                  <- index of canonical queries
    <query_1>.tql              <- composed views with parameters
    <query_2>.tql
    ...
  context/
    <table_or_concept>.md      <- gotchas, conventions, anything not in .tql
    ...
```

For each file type, point me to the matching skeleton in `templates/`.

## Step 10 - Inside-the-file structure

For each `.tql` object, the standard contents are:

1. `params` block (metrics, dimensions, filters this object exposes)
2. `metric_keys` and `dimension_keys` type sets
3. `metrics` declarations (the aggregations)
4. `dimensions` declarations (the group-by columns)
5. The backing source (`source` - either a table or a query)
6. Optionally `needs` and `join_if` helpers for composability

For each context `.md` (the gotchas), the standard sections are:

1. Title and one-line purpose
2. Source / object cross-reference
3. Schema table (for tables that need it)
4. Critical gotchas (the top 3)
5. Timezone / convention notes
6. Common joins
7. Cross-references (link to the `.tql` queries that use this object)

Show me the draft for the first object before generating the rest. Get my approval on shape, then produce the others.

## Step 11 - The README at the top

The source's `README.md` is the routing entry. It should have:

1. One-paragraph purpose
2. Connection info (name, type, access mode)
3. A routing table with the top 10 questions and which query / object each maps to
4. An "objects of interest" table linking to each `.tql`
5. A "queries of interest" table linking to the query catalog
6. Pointers to related folders (relationships, business context, the team hub)

## Step 12 - Update the relevant nav table(s)

Add or update entries in the relevant nav table(s) so this slice becomes discoverable:

- If this slice is **org-wide** (every user should be able to reach it), add a row in `shared/ROUTING.md`.
- If this slice is **team-scoped** (only one team's users should reach it), add a row in that team's `<team>/ROUTING.md`.
- If this slice is **connector-scoped** (only loads when the connector is active), add it to the connector's nav table.
- If a target nav table does not exist yet, this is a signal to run `recipe-01-nav-tables.md` after this one.

The added rows should be `trigger phrase -> path/to/file.md` entries, not prose definitions.

> **Manual UI step**: If you create a new nav table here, remember it does not auto-attach automatically. An admin has to bind it to the right scope (org, role, connector) in the TextQL app UI before the auto-attach takes effect.

## Step 13 - Sense-check

Before opening the PR:

- Does every object have its PK / dimensions / measures classified?
- Does every metric have its filters and time grain documented (not assumed)?
- Do the canonical queries actually run and return the right numbers?
- Have I excluded test data / employee data / soft-deletes consistently?
- Have I documented the timezone convention?
- Does the README route the top 10 questions to the right file?

Only then produce the patch.
