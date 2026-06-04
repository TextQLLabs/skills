# <Database Name>

> **Connector**: <connector display name> (<engine>, schema `<schema>`, <access mode>)
> One-paragraph description of the domain this database covers and the canonical questions it answers.

## How to query this database

**Always prefer the parameterized queries in [`queries/`](queries/) over hand-written SQL.** They encode the conventions the team has learned (exclusions, timezone, gotchas).

See [`queries/README.md`](queries/README.md) for the full catalog.

## High-value entry points

| Question | Query |
|---|---|
| "<question 1>" | [`<query_1>.sql`](queries/<query_1>.sql) |
| "<question 2>" | [`<query_2>.sql`](queries/<query_2>.sql) |

## Tables with non-trivial schema or guardrails

| Table | Why you would read the doc |
|---|---|
| [`<table_1>`](tables/<table_1>.md) | <gotcha or convention> |
| [`<table_2>`](tables/<table_2>.md) | <gotcha or convention> |

## Cross-database relationships

If this database joins to others, see [relationships.md](relationships.md).

## See also

- [<root routing file>](<path>) - top-level routing
- [<related business context>](<path>) - definitions of the entities this database stores
