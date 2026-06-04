# Table: `<table_name>`

> One-sentence description of what each row represents.

**Database**: <database name> (Connector ID <id>)

> Cross-reference callout. Example: To convert <X> to <Y>, use `<other_table>`. Do **not** assume a flat <value> from `<wrong_table>`. See [<other_table>](<path>).

## Schema

| Column | Type | Nullable | Default | Description |
|---|---|---|---|---|
| `<col_1>` | `<type>` | <NOT NULL / YES> | `<default>` | <description> |
| `<col_2>` | `<type>` | <NOT NULL / YES> | -- | <description> |

## Critical gotchas

1. **<Gotcha 1>**: <Why people get this wrong; what to do instead>
2. **<Gotcha 2>**: <Why people get this wrong; what to do instead>

## Timezone / convention

All timestamps stored in <UTC / local zone>. When filtering by date, <how to handle>.

## Common joins

- `<this_table>.<col>` <-> `<other_table>.<col>` (<one-line semantics>)
- `<this_table>.<col>` <-> `<other_table>.<col>` (<one-line semantics>)

## Exclusions

When counting "real" rows, exclude:

- <Exclusion 1>: <reason>
- <Exclusion 2>: <reason>

## See also

- [<query catalog README>](<path>) - canonical queries that use this table
- [<related table>](<path>) - <relationship>
