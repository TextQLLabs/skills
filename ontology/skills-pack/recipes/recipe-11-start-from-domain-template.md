# Recipe: Start From a Prebuilt Domain Template

> **How to use this file**: Copy everything below the `---` and paste it as your message in a new chat. Ana will guide the rest.

---

My organization is in a domain that may already have a prebuilt TextQL ontology template (for example healthcare / life-sciences, or financial services). Rather than build from scratch, I want to start from that template, connect it to my data, validate it, and adapt it to my organization. Please run this as a guided session - ask one question at a time and wait for my answer before producing files.

A domain template ships the parts that are the same for everyone in the domain - the entity spine, governed metric surfaces, code-set / terminology crosswalks, governance defaults, and validated golden queries - so I **adapt rather than author**. This is hours, not weeks, and the starting point is already validated.

## Step 1 - Identify the domain and the right template

Ask me what domain my data is in, then point me to the matching template repo. Known templates:

- **Healthcare & life sciences** (payers, providers, PBMs) -> `TextQLLabs/ontology-healthcare-starter`
- *(more domains as they are published - each ships its own `SKILL.md` so it's discoverable)*

If no template covers my domain, stop here and recommend `recipe-00-scoping-conversation.md` to build from scratch with the recipes instead. Don't force a mismatched template.

## Step 2 - Connect the template as context

Walk me through connecting the template repo to Ana via the **Git connector**, so Ana reads the whole ontology (metrics, notes, terminology, governance) as ground truth. Confirm Ana can see the repo's routing / `SKILL.md` file and has loaded the structure.

## Step 3 - Connect my data (read-only)

Have me connect the warehouse that holds my data. **Read-only access is enough** - a good template is designed not to require any writes (terminology joins run in Ana's Python sandbox). Note the dialect (Redshift / BigQuery / Snowflake) and the connector.

## Step 4 - Validate the template against my schema

This is the key step. The template assumes certain table and column names; mine will differ. Use the template's dry-run playbook (e.g. `validation/dry-run-prompt.md`): pull my `information_schema`, diff it against the template's expected table backings, and list every mismatch (missing tables, renamed columns, different code columns).

## Step 5 - Adapt the backings (open a PR)

For each mismatch, propose the exact edit to the template's `schema.tql` - the single place table backings live - plus any column references in the affected relations/queries. Make the edits on a branch and **open a PR I review**; do not push to main. Repointing the backings should cascade to every downstream metric.

## Step 6 - Confirm the metrics and terminology work

Run the template's golden queries against my data and compare to the pinned values (adjusting for my population size). Confirm the terminology / code-set joins resolve. Flag any surface that needs a definition change for my org before I rely on its number.

## Step 7 - Make it mine

Ask which template defaults differ from my organization's reality:
- A metric defined differently than we define it (e.g. cost on allowed vs. billed amount)?
- A code set or value set we license or define our own way?
- Governance rules we must tighten (access, residency, suppression)?

For each, queue a follow-up using the sibling recipes (`recipe-06` for a definition, `recipe-03` for a new source) and record the decision in the template's `notes/`.

## Step 8 - Update the relevant nav table(s)

Add or update entries so the adapted slices stay discoverable:
- Org-wide reach -> a row in `shared/ROUTING.md`. Team-scoped -> that team's `<team>/ROUTING.md`. Connector-scoped -> the connector's nav table.
- Rows are `trigger phrase -> path/to/file.md`, not prose.

> **Manual UI step**: a new nav table does not auto-attach. An admin must bind it to the right scope (org, role, connector) in the TextQL app UI.

## Step 9 - Sense-check

Before relying on it:
- Is every table backing repointed to a real table in my warehouse?
- Do the golden queries run and return sensible numbers for my population?
- Are the governance defaults at least as strict as my org requires (never looser)?
- Is every adaptation captured as a reviewed PR with a note explaining why?

Then proceed. From here, extend with the other recipes - the template is your foundation, not a finished product.
