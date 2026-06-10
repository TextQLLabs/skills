# Recipe: Author a `.tql` File (guided session + syntax reference)

> **You do not need to know what a `.tql` file is to use this.** This recipe runs a guided session where you describe - in plain business terms - the numbers you want and the tables they live in. Ana figures out the rest (including which `.tql` shape to write) and walks you through it.

> **Source of truth**: the official **[.tql Manual](https://docs.textql.com/core/ontology/tql-reference)**. The back half of this file is a condensed, copy-pasteable summary of that manual; when the two disagree, the manual wins.

> **How to use this file**: copy everything below the first `---` into a new chat. Ana will ask a handful of plain-English questions and produce the `.tql` file for you. The "Condensed syntax reference" at the bottom is for Ana and for anyone reviewing a draft - you don't need to read it first.

> *What a `.tql` file is, in one sentence (for the curious): it's a reviewable file that defines a metric/query once - the formula, the tables, the joins, the filters - so everyone gets the same governed answer instead of re-deriving SQL each time.*

---

I want to turn some of my team's numbers into `.tql` files so Ana computes them consistently. I may not know anything about the `.tql` format yet - please run this as a guided session, ask one plain-English question at a time, wait for my answer, and **you decide the technical shape of the file(s) for me.** Do not ask me to choose a file shape, a "semantic view vs plain query," or any `.tql` construct. Infer all of that from my answers and explain your choice in plain language when you draft.

## Step 1 - What do you want to measure?

This is the most important step. Ask me to describe, in plain business language, the metric(s) or question(s) I want to capture. Encourage me to just paste things in - I can drop in any of:
- A sentence like "monthly revenue per customer" or "active users in the last 30 days"
- An existing SQL query I run by hand
- A screenshot or export of a report/dashboard
- A spreadsheet formula or a Slack message where someone explained the number

For each metric I mention, draw out (one question at a time, in everyday terms):
- **What it counts or sums** - "is this a dollar amount, a count of things, an average, a rate?"
- **The plain-English definition** - "how would you explain this number to a new hire?"
- **What to leave out** - "should this exclude test accounts, internal employees, cancelled/refunded rows, deleted records?"
- **The time window** - "per day? per month? rolling 30 days? and is the clock UTC or a local timezone?"
- **Who owns the definition** - "if two people disagreed on this number, who decides?"

Capture these as prose first. The SQL comes later.

## Step 2 - Where does the data live? (tables)

Ask me which tables hold the data behind these numbers. For each table, in plain terms:
- Its name (schema-qualified if I know it, e.g. `shop.orders`)
- **What one row represents** - "one row per order? per customer? per event?"
- Which columns carry the numbers I want to measure
- Which columns I'd use to slice or filter (region, status, date, customer name)

If I only have one table, that's fine - say so and move on. If I'm not sure of exact names, ask me to paste a few sample rows or column names, or offer to look at the connector's schema.

## Step 3 - How do the tables connect? (joins)

Only if Step 2 surfaced more than one table, ask me how they relate, in plain terms:
- "Which column in table A matches which column in table B?" (the **join keys**, e.g. `orders.customer_id = customers.id`)
- "For one row in A, how many rows in B?" (one-to-one, one-to-many, many-to-many) - this matters because a careless join can double-count a sum
- Whether any join is a "bridge"/lookup table sitting between two others
- Whether a table should only be joined *sometimes* (e.g. "only when I break out by sales rep")

Note any join key back to me so I can confirm it's right before you write anything. If a join could inflate a SUM (many-to-many, or fact-to-fact at different grains), flag it and ask how I want to handle it.

## Step 4 - How will people ask for it? (the inputs)

Ask what should be adjustable each time the number is pulled - these become the file's parameters:
- A date range or lookback window?
- A specific customer / region / segment filter?
- An optional breakout (e.g. "by month", "by rep") that's sometimes on, sometimes off?
- Anything that must always be provided (no sensible default)?

For each, get a one-line purpose and 2-3 example values. Keep it conversational - "what would you want to change between two runs of this report?"

## Step 5 - Ana picks the shape and drafts (your job, not mine)

Now YOU choose the `.tql` shape from what I told you, and say which you picked and why in one plain sentence. Rules of thumb:
- One table, a few fixed inputs, no optional joins -> **plain template query**.
- Several governed metrics/dimensions the caller selects from, or filters that vary -> **semantic view** (`metrics`/`dimensions`/`filters` with `matchSet` + `filterWhere`).
- A join (or column, or whole table) that should only appear when a certain breakout/filter is requested -> **conditional joins** in an expression body.
- The same backing table or metric set will be reused across several files -> factor shared pieces into `objects/`, `relations/`, or `_defs/` and compose them (fact + dimension pattern).

Draft the **top-of-file comment + params block first** (the contract), show me just that much in plain terms, and get my sign-off before writing the SQL body. Then fill in the body.

## Step 6 - Test before declaring done (NON-NEGOTIABLE)

Per the manual, never call a `.tql` file finished until you've:
1. **`inspect`** - confirm the inputs parse with the right types/defaults and that each has a doc comment.
2. **`render`** - show me the generated SQL for the default inputs AND for each optional branch (with/without each breakout, filter, join) so I can eyeball it.
3. **`execute`** - run it and confirm the number matches what I expect (sanity-check against a known value if I have one).
4. If other files import this one, render one of them so the import still resolves.

Walk me through the rendered SQL in plain language - I should be able to confirm the definition is right without reading `.tql` syntax.

## Step 7 - Make it findable and write down the meaning

- Add a `-- See also:` block linking related `.tql` files and a plain-English `.md` note for any metric (business meaning, exclusions, time grain). The `.tql` and the `.md` should point at each other.
- Add a routing row in the relevant `ROUTING.md` / database `README.md` so the file is discoverable by the phrases I'd actually type.
- **Wire up the auto-attached file.** Check whether the auto-attached file for this scope (the `ROUTING.md` or `Ana.md` that rides along in every relevant chat) already has an entry pointing to this routing file. If not, add one. A `.tql` file that lives behind two levels of routing is only reachable if every link in the chain is wired — if the auto-attached file doesn't reference the routing file, queries that should find this `.tql` never will. The entry can be a single row: `"<trigger phrase for this metric/query>" -> <path/to/ROUTING.md or database README>`.
- Offer to capture anything I got wrong or surprising as a gotcha note.

# Condensed syntax reference

> This mirrors the **[.tql Manual](https://docs.textql.com/core/ontology/tql-reference)**. For the full treatment - operator precedence table, every builtin, the complete fact/dimension walkthrough, and rendered-SQL examples - read the manual.

## The two file forms

**Plain SQL template** (param-name interpolation only):

```tql
params {
  -- Country code. Examples: "US", "CA", "BR"
  country: String = "US"
  -- Inclusive lower bound, ISO 8601 timestamp.
  created_after: Timestamp?
}

SELECT *
FROM visits
WHERE country = ${country}
  AND (${created_after} IS NULL OR created_at >= ${created_after})
```

**Expression body** (branching, fragments, imports, semantic views):

```tql
params { include_region: Bool = false }
let
  select_expr  = if include_region then sql"region, COUNT(*)" else sql"COUNT(*)"
  group_clause = if include_region then sql"GROUP BY region" else sql""
in sql''
  SELECT ${select_expr}
  FROM orders
  ${group_clause}
''
```

## Param type system

| Type | JSON | Notes |
|---|---|---|
| `Int` | integer | non-integers rejected |
| `Float` | number | integers accepted |
| `String` | string | literal values, not identifiers; **double-quote defaults** |
| `Bool` | boolean | use with `if` / boolean ops |
| `Date` | string | document the format (not deeply parsed) |
| `Timestamp` | string | document timezone |
| `Set<"a"\|"b">` | array of strings | deduped + validated against allowed labels |
| `List<T>` | array | validated recursively |
| `FilterInput` | object | usually `List<FilterInput>` |

`?` = nullable (omitted -> `null`). Non-nullable without default = required. Defaults allowed for scalar literals and empty lists/sets. Params are **newline-separated, no commas**; equality is `==`/`!=`, never bare `=`.

**Pure expressions in `Set<>` signatures** (key multi-file technique):

```tql
-- objects/orders.tql
let metric_keys = ["revenue","order_count"]
    dimension_keys = ["Orders.customer","Orders.month"]
in { metric_keys, dimension_keys }

-- queries/orders.tql
import orders from "../objects/orders.tql"
params {
  metrics: Set<orders.metric_keys> = []
  dimensions: Set<orders.dimension_keys ++ ["Orders.region"]> = []
}
```

## Imports

```tql
import t from "../relations/transactions.tql"      -- whole-record import
import { eq } from "../filters/compare.tql"         -- destructured import
```

Relative paths resolve from the importing file; absolute from project root.

## Interpolation rules (the ones that bite)

- `${expr}` -> scalars become bind/escaped values, `null` -> `NULL`, lists -> SQL tuples, `SqlFragment` -> spliced directly.
- **Never quote interpolations**: `WHERE c = ${c}`, never `'${c}'`.
- **Lists already add parens**: `WHERE id IN ${ids}`, never `IN (${ids})`.
- Nullable params are NOT auto-omitted: `col >= ${t}` with null `t` renders `col >= NULL`. Branch with `if t == null then sql"" else ...`.

## Operators

Boolean `||` `&&` `not` | equality `==` `!=` | comparison `> < >= <=` | concat `++` (two fragments or two lists). Precedence high->low: field access / application, `not`, `++`, comparisons, equality, `&&`, `||`. Parenthesize mixed boolean ops.

## Builtins

| Builtin | Purpose |
|---|---|
| `concatSep` | join fragments with a separator (skips empties) |
| `wrap` | add prefix/suffix only if fragment non-empty |
| `isEmpty` | true for empty fragment/string/set/list/null |
| `map` | apply a function to each list item |
| `any` | true if any item in a bool list is true |
| `contains` | membership in a set or list |
| `matchSet` | map a Set param to authored arms, in authored order |
| `filter` | keep list items where a predicate is true |
| `dedupeByKey` | dedupe record lists by their `key` field (first wins) |
| `filterWhere` | compile `FilterInput`s against an authored `filterables` registry |
| `error` | stop rendering with a message |

## Expressions

Literals (numbers, strings, bools, `null`, lists, records); `let ... in ...` (ordered, no duplicate names); records `{ expr = sql"...", join = sql"..." }` with shorthand `{ expr, join }`; field access `dims.buyer.expr`; `if c then a else b` (only selected branch evaluates); lambdas `\col val -> sql"${col} = ${val}"` (whitespace application); `matchSet set { "k" -> ... }`.

## Semantic view (Manual Pattern 2 - condensed)

```tql
params {
  metrics: Set<"revenue" | "order_count"> = []
  dimensions: Set<"customer" | "month"> = []
  filters: List<FilterInput> = []
}
let
  metric_frags = matchSet metrics {
    "revenue"     -> sql"SUM(o.revenue) AS revenue"
    "order_count" -> sql"COUNT(DISTINCT o.id) AS order_count"
  }
  dim_entries = matchSet dimensions {
    "customer" -> { expr = sql"c.name", join = sql"JOIN customers c ON o.customer_id = c.id" }
    "month"    -> { expr = sql"DATE_TRUNC('month', o.ordered_at)", join = sql"" }
  }
  filterables = [
    { key = "customer_name", expr = sql"c.name",      ops = ["equals","like"] }
  , { key = "ordered_at",    expr = sql"o.ordered_at", ops = ["gte","lte","between"] }
  ]
  joins          = concatSep " " (map (\d -> d.join) dim_entries)
  select_dims    = concatSep ", " (map (\d -> d.expr) dim_entries)
  select_metrics = concatSep ", " metric_frags
  select_expr = if isEmpty select_dims && isEmpty select_metrics then sql"*"
    else if isEmpty select_dims then select_metrics
    else if isEmpty select_metrics then select_dims
    else sql"${select_dims}, ${select_metrics}"
  where_clause = wrap "WHERE "    "" (filterWhere filterables filters)
  group_clause = wrap "GROUP BY " "" select_dims
in sql''
  SELECT ${select_expr}
  FROM orders o
  ${joins}
  ${where_clause}
  ${group_clause}
''
```

## FilterInput

JSON: `{ "key": "...", "operator": "...", "value": ... }` (or `"values": [...]` for `in`/`between`; `op` is accepted as an alias of `operator`). Canonical operators: `equals`, `not_equals`, `gt`, `gte`, `lt`, `lte`, `like`, `not_like`, `starts_with`, `not_starts_with`, `ends_with`, `not_ends_with`, `between`, `in`, `not_in`, `is_null`, `is_not_null` (aliases like `eq`, `ilike`, `greater_than_or_equal` are normalized). Each filterable's `ops` list still gates which operators its key allows.

## Runtime context (`_tql`, no declaration needed)

- `_tql.roleset_by_rolename` - set of effective chat role names
- `_tql.role_names` - list of the same
- `_tql.client_attributes_json` (alias `_tql.client_attributes`) - client attribute object

**Fail-closed RLS** (Manual Pattern 5): never silently render unscoped SQL.

```tql
let
  tenant_id  = _tql.client_attributes_json.tenant_id
  has_access = any (map (\r -> contains _tql.roleset_by_rolename r) ["tenant_admin","tenant_viewer"])
  scoped     = if has_access then tenant_id else error "Query requires tenant_admin or tenant_viewer"
in sql"SELECT * FROM accounts WHERE tenant_id = ${scoped}"
```

## Fact + dimension star schema (Manual Pattern 4 - shape only)

Object modules export `backing`, `source`, `metric_keys`, `dimension_keys`, `filter_keys`, `semantic_keys`, `needs`, `join`/`join_if`, `metrics`, `dimensions`, `filterables`. Query entrypoints `import` the objects, declare `Set<obj.metric_keys>` / `Set<obj_a.dimension_keys ++ obj_b.dimension_keys>` params, and compose with `dedupeByKey` + `filter` + conditional `join_if`. See the manual's Pattern 4 for the full two-object walkthrough and rendered SQL - it is long and worth reading in full rather than reproducing here.

## Shared `_defs` fragment

```tql
-- _defs/order_status_label.tql -- SOURCE OF TRUTH for the status CASE. Callers alias the table as `o`.
params {}
let status_label = sql''CASE o.status WHEN 'paid' THEN 'Paid' WHEN 'refunded' THEN 'Refunded' ELSE 'Open' END''
in { status_label = status_label }
```

```tql
import status_def from "./_defs/order_status_label.tql"
params { }
let label = status_def.status_label
in sql''SELECT o.order_id, ${label} AS status_label FROM orders o''
```

## Checklist (from the manual)

- [ ] `params { ... }`, one declaration per line, no commas; `--` docs above each.
- [ ] String defaults double-quoted; equality `==`/`!=`, not bare `=`.
- [ ] Optional clauses via `if` / `isEmpty` / `wrap`; no interpolation wrapped in SQL quotes; list interpolations not wrapped in extra parens.
- [ ] Metrics aliased with `AS stable_name`; filter params document allowed keys + operators; filtering via `filterWhere`, not caller SQL.
- [ ] Conditional joins include every dimension and filter key that requires them.
- [ ] Runtime access checks fail closed with `error`; shared backings live in a local `schema.tql` / `_defs/*.tql`.
- [ ] File has been **inspected and rendered** (and ideally executed) before approval.
