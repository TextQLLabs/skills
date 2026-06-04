# Recipe: Document an API Integration

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to document one external API integration so Ana knows how to call it correctly. Please run this as a guided session.

## Step 1 - Identify the API

Ask me:
- The name of the service
- The base URL
- The auth method (API key, OAuth, service account, header-based, signed requests)
- Whether the auth credential is per-user or org-wide
- The rate limit (per minute, per hour, per day)
- The official docs URL (if any)
- Whether this API is read-only for my purposes, write-mostly, or both

## Step 2 - Decide: API vs warehouse mirror

This is a critical decision that many teams get wrong. Ask:

- Is the data in this API also synced to a database I already query?

If yes: **Reads should go to the database. The API is for writes only.**

This matters because most CRMs and SaaS APIs have low pagination limits (50-100 records per page). Reading any non-trivial dataset through them is slow and easy to mistake. A warehouse mirror returns everything in one query.

Document this routing rule prominently at the top of the README. Example:

> **This API is for WRITES.** For reads, use `<database location>`. The warehouse mirror has no pagination limit and is far faster.

## Step 3 - List the operations

Ask me what my team actually does with this API. For each operation, get:

- Plain English description ("create a deal", "send a Slack message", "add a comment to a page")
- HTTP method and endpoint
- A minimal working example payload
- Required fields vs optional fields
- What success looks like in the response

If my team only writes (never reads) or only reads (never writes), document only the operations actually in use. The library is not a clone of the official API docs.

## Step 4 - Write-safety rules

If the API can mutate data, draft a write-safety section. This is non-negotiable for any write API. Include:

- **Pre-write checklist.** Before any write, what must Ana verify? (e.g., "search by company name not deal name to avoid duplicates")
- **Closed-deal protection.** Are there records that should never be modified once in a terminal state?
- **Ambiguity escalation.** When the input is ambiguous (multiple matches, missing required fields), Ana must confirm with the user before writing.
- **Confirmation rules.** For destructive writes (delete, archive), require explicit user confirmation.
- **Idempotency.** If a write fails halfway, is it safe to retry?

A dedicated `write-safety-guidelines.md` is appropriate when the API supports many destructive operations.

## Step 5 - Gotchas and edge cases

Ask me: what are the things this API does that surprise people?

- Fields that are required by the docs but actually optional (or vice versa)
- Rate limit headers and how to back off
- Error responses that look like success
- IDs that look identical but mean different things (record_id vs object_id vs slug)
- Pagination cursors that aren't what you'd expect
- Webhook delivery patterns (if relevant)

## Step 6 - Folder layout

Recommend this structure:

```
apis/<service_name>/
  README.md                    <- overview, auth, rate limits, routing rule
  <service>_api_guide.md       <- the consolidated how-to
  write-safety-guidelines.md   <- if the API has writes
  <specific_workflow_1>.md     <- e.g., creating_deals.md
  <specific_workflow_2>.md     <- e.g., updating_records.md
  url_formatting.md            <- if URL construction is non-trivial
```

For each, point me to:
- `templates/api-readme.md` for the README

## Step 7 - Inside-the-file structure

The README's job is *routing*, not exhaustive content. Standard sections:

1. **Title and purpose** ("This is the [Service] API. It's used for [purpose]")
2. **Auth snippet** (a code block showing how to authenticate)
3. **Base URL and rate limit**
4. **The READ/WRITE routing rule** (where reads should go vs writes)
5. **MANDATORY before any write** (link to write-safety doc)
6. **Where each operation lives** (small table mapping operation -> doc)
7. **Key endpoints** (table of method + path + purpose)

The detailed how-to lives in the sibling files, not the README.

## Step 8 - Cross-references

Identify which other docs in the library should be linked:

- Any database that contains the same data (the read alternative)
- Any business context that defines the entities (e.g., if you're documenting a CRM API, link to the deal-stages and deal-fields docs)
- Any workflows that call this API
- The relevant nav table(s) (`shared/ROUTING.md` for org-wide APIs, or a team's nav table if the API is team-scoped)

## Step 9 - Sense-check

Before producing the patch:

- Have I made the read-vs-write routing rule prominent?
- Is the write-safety section concrete and testable, not vague?
- Does the auth snippet work as-is, with the right credential placeholder?
- Does the README's routing table cover the top 5 operations?
- Have I avoided duplicating the official API docs?

Only after this checklist, produce the patch.
