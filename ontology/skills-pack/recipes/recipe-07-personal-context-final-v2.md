# Recipe: Build a Personal Context Profile

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

I want to build (or update) my personal context profile. This is the file Ana reads at the start of every conversation to know how to talk to me. Please run this as a guided session.

## What personal context IS

- Your voice-to-text quirks and common typos
- Your preferred response style (concise vs verbose, with charts vs without, with SQL shown vs hidden)
- Tools, dashboards, and reports you use weekly
- Recurring workflows you run
- Time zone and working hours preferences
- Topics you frequently come back to

## What personal context is NOT

- Facts about you for other people to read (your title, your projects, your bio). Personal context is "how Ana talks to *you* when *you* are the user." If a colleague asks "who is <me>?", they should not be reading my personal context file.
- Sensitive credentials, passwords, or tokens. Those go in a credentials manager.
- Information about colleagues that they have not consented to share.

## Step 1 - Communication style

Ask me about how I interact with Ana:

- Do I primarily type or dictate?
- Do I prefer concise answers (1-2 sentences, just the answer) or detailed walk-throughs?
- Do I want SQL/code shown in responses, or just the result?
- Do I prefer charts/visualizations by default, or only when asked?
- Do I tend to ask multi-part questions, or single questions?
- Do I have a tone preference (formal, casual, professorial, terse)?

Capture these as a small "Preferred Response Style" section.

## Step 2 - Common misspellings and shorthand

If I dictate or type quickly, I almost certainly have a personal vocabulary of typos and shorthand. Ask me to volunteer the ones I'm aware of. Then offer to learn more over time and add them later.

Standard format - a table:

```
| What I say / type | What I mean | Frequency |
|---|---|---|
| ...               | ...         | Common    |
```

## Step 3 - Names I use for things

People rarely call systems by their official names. Capture mine:

- Internal nicknames for products, tools, dashboards
- Nicknames for colleagues (especially if I use first names that have multiple matches in the team)
- Short forms for common phrases ("EOD" = end of day, "MTD" = month to date, etc.)

## Step 4 - Workflows I run

Ask me what I do regularly. Examples:

- "Every Monday morning I want a forecast update"
- "When I say 'do my weekly review,' run X, Y, Z"
- "I always need the timezone in <my zone> for date filters"

These become entries in a "My Workflows" section. If any of them deserve to be promoted to a team-wide workflow doc, note that too.

## Step 5 - Topics I frequently work on

Ask me what I work on most. Examples:

- "I focus on the finance team's monthly close"
- "I own the data quality of the CRM"
- "I'm an engineer working on the data pipeline"

This helps Ana bias toward relevant docs when my question is ambiguous.

## Step 6 - Personal artifacts

If I've already built dashboards, playbooks, or reports, capture them so Ana can find them quickly. Standard sub-folders under my user directory:

```
users/<my_local_part>/
  context.md         <- the main personal context file
  dashboards/        <- references to my dashboards
  workflows/         <- my personal workflows
  outreach/          <- saved prospecting / contact lists (if relevant)
```

If I don't have any of these yet, leave the sub-folders out for now.

## Step 7 - Notification preferences

Ask:

- Where do I want notifications? (Slack DM, email, both, neither)
- Which Slack channel am I most active in (so Ana can default to that for shared notifications)?
- Are there times of day or days of week when I do not want to receive automated reports?

## Step 8 - Things to keep updating

End the file with a note to my future self: this is a living document. Ana should ask "should I add this to your personal context?" when it notices new patterns. The right answer is yes.

## Step 9 - Draft

Standard structure:

```
# <My Name> - Personal Context

> Email: <my email>
> Role: <my role>
> Last updated: <date>

## Communication Style
...

## Common Misspellings (Voice-to-Text)
| What I say | What I mean | Frequency |
| ...        | ...         | ...       |

## Names I use for things
- ...

## Topics I frequently work on
- ...

## My Workflows
- "When I say X, do Y"
- ...

## Personal Artifacts
- Dashboards: ...
- Playbooks: ...

## Notification Preferences
- ...

## Things to keep updating
This file is a living document. Add to it when you notice new patterns.
```

Show me the draft. Walk through. Let me edit.

## Step 10 - Folder placement

Personal context lives at:

```
users/<my email local part>/context.md
```

For example, if my email is `alice@example.com`, then `users/alice/context.md`.

Not nested under a team. Personal context is about *the person*, not the team they're currently on. (If I am on multiple teams or switch teams, the file stays.)

## Step 11 - Sense-check

Before producing the patch:

- Is the file readable as if a colleague were learning how to work with me?
- Have I avoided storing sensitive credentials?
- Have I avoided describing colleagues in ways they wouldn't recognize or approve of?
- Is there a clear "communication style" section so Ana doesn't have to guess?

Then produce the patch. After it's in, Ana will load this file at the start of every conversation going forward.
