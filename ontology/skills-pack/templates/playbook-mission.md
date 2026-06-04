# Playbook: <Name>

## Purpose

<One paragraph: what business question this answers and why.>

## Audience

<Who reads this report and what decision it informs.>

## Schedule

- **Cron**: `<cron expression>`
- **Cadence**: <human description, e.g., "every weekday at 9am ET">
- **Why this cadence**: <reason>

## Destination

- **Slack channel**: `<channel>`
- **Email recipients**: <list or "none">

## Inputs

| Source | Used for |
|---|---|
| <database / API> | <which numbers> |
| <database / API> | <which numbers> |

## Outputs

<Describe the report structure: sections, charts, narrative.>

1. Section 1: <description>
2. Section 2: <description>
3. Chart 1: <description>

## Methodology

<The calculations and any non-obvious choices. Link to canonical metric definitions rather than redefining.>

- Metric A: defined in [<path>](<path>)
- Metric B: defined in [<path>](<path>)
- Filter rule: <description>
- Timezone: <UTC / local zone>

## Edge cases

- **Weekends / holidays**: <behavior>
- **Missing data**: <behavior>
- **Schema changes**: <how the playbook should fail safely>

## Templatization (if applicable)

If this playbook is templatized (same code, many instances), document the parameters:

| Parameter | Type | Required | Default | Example |
|---|---|---|---|---|
| `<param_1>` | <type> | <yes/no> | `<default>` | `<example>` |

## See also

- [REFERENCES.md](REFERENCES.md) - cross-reference to underlying queries, APIs, helpers
- [<related metric>](<path>) - definition of the key number this playbook reports
