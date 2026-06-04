# <Database Name> Queries

> Catalog of canonical queries for <database>. Each query encodes the right exclusions, joins, and timezone conventions.

## Usage

Run via the platform's parameterized query tool. The file header documents each parameter.

## Catalog

### <Group 1: e.g., Daily aggregates>

| File | What it answers | Parameters |
|---|---|---|
| `<query_1>.sql` | <question> | `<param>` |
| `<query_2>.sql` | <question> | `<param>` |

### <Group 2: e.g., Per-customer breakdowns>

| File | What it answers | Parameters |
|---|---|---|
| `<query_3>.sql` | <question> | `<param>` |

## When to write a new query

Add a new canonical query when:

1. The same question gets asked more than twice
2. The right SQL requires a non-obvious exclusion or join
3. The result drives a recurring decision

Don't add a query for one-off explorations.

## See also

- [<database README>](README.md) - database overview
- [<tables README>](tables/README.md) - table schemas
