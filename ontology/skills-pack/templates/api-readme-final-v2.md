# <Service> API

> **API access ID**: `<id>`
> **Auth header**: `<auth header pattern>`
> **Base URL**: `<base url>`
> **Rate limit**: <rate limit>

**This API is for <reads / writes / both>.** <If reads also live in a warehouse mirror, link it here and note the warehouse is faster.>

## MANDATORY before any write

1. Read [write-safety-guidelines.md](write-safety-guidelines.md) - <one-line summary>.
2. <Pre-write check 1>
3. <Pre-write check 2>
4. <Confirmation rule>

## Where each operation lives

| Operation | Use | Doc |
|---|---|---|
| Read records in bulk | <Database / API> | [<path>](<path>) |
| Create a record | API | [creating_records.md](creating_records.md) |
| Update a record | API | [updating_records.md](updating_records.md) |

## Auth snippet

```python
TOKEN = '<credential placeholder>'
headers = {
    'Authorization': f'Bearer {TOKEN}',
    'Content-Type': 'application/json',
}
base_url = '<base url>'
```

## Key endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `<path>` | <purpose> |
| `POST` | `<path>` | <purpose> |
| `PATCH` | `<path>` | <purpose> |

## Common gotchas

1. **<Gotcha 1>**: <description>
2. **<Gotcha 2>**: <description>

## See also

- [<related database / warehouse mirror>](<path>) - reads should go here
- [<related business context>](<path>) - what the entities mean
