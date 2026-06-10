# `.tql` file skeleton

> Source of truth: the [.tql Manual](https://docs.textql.com/core/ontology/tql-reference). These are quick skeletons; the manual has the full spec.

Pick the shape that matches the file. All examples use a generic `shop` schema - substitute your own. See `recipes/recipe-11-tql-authoring.md` for the full syntax reference and the test loop.

## Plain parameterized query

```tql
-- <One-line purpose: what one row represents, and the question it answers.>
-- Database: <schema> on <connector name>.
--
-- See also:
--   <related_query>.tql        -- <relationship>
--   context/<concept>.md       -- business meaning + gotchas

params {
  -- <Purpose>. Pass 0 to disable the cap. Examples: 7, 30, 90
  lookback_days: Int = 30

  -- <Purpose>. Null = no filter. Examples: "<ex1>", "<ex2>"
  some_filter: String?
}

SELECT
    <dimension>,
    <AGG>(<measure>) AS <stable_name>
FROM <schema>.<table> t
WHERE (${lookback_days} <= 0 OR t.<ts_col> >= CURRENT_DATE - (${lookback_days} || ' days')::interval)
  AND (${some_filter} IS NULL OR t.<col> ILIKE '%' || ${some_filter} || '%')
GROUP BY <dimension>
ORDER BY <stable_name> DESC;
```

## Expression-style composed view (conditional joins/columns)

```tql
-- <One-line purpose.>
params {
  -- <Purpose>. Null = all.
  name_filter: String?
  -- Breakout dimensions. Empty = base grain. Allowed: "<dim>".
  breakout: Set<"<dim>"> = []
}

let
  include_x  = contains breakout "<dim>"
  x_select   = if include_x then sql", j.<col> AS <alias>" else sql""
  x_join     = if include_x then sql"LEFT JOIN <schema>.<other> j ON t.<fk> = j.<pk>" else sql""
  x_group    = if include_x then sql", j.<col>" else sql""
in sql''
SELECT t.<dimension> ${x_select}, <AGG>(t.<measure>) AS <stable_name>
FROM <schema>.<table> t
${x_join}
WHERE (${name_filter} IS NULL OR t.<col> ILIKE '%' || ${name_filter} || '%')
GROUP BY t.<dimension> ${x_group}
ORDER BY <stable_name> DESC
''
```

## Shared `_defs` fragment

```tql
-- _defs/<name>.tql -- SOURCE OF TRUTH for <what>. Callers alias the table as `t`.
-- Usage: import <alias> from "./_defs/<name>.tql"; ... ${<alias>.<export>} ...
params {}

let
  <export> = sql''<reusable SQL fragment>''
in { <export> = <export> }
```
