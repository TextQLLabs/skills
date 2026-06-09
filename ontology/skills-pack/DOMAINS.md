# Domain Ontology Starters — catalog

The prebuilt ontologies the `ontology-builder` skill routes to (Path 1 in `SKILL.md`). Each registers
via its own `SKILL.md` so it's discoverable. Connect the one that fits, validate against your data,
adapt.

**Status:** ✅ validated on live data · 🟡 v1 (structure complete, not yet validated) · ⚪ planned

## Industry verticals — full prebuilt starters
Regulated/standardized domains where a heavy, pre-modeled starter pays off (entity spine + governed
metrics + code-set crosswalks + governance + golden queries).

| Domain | Repo | Status | Highlights |
|---|---|---|---|
| Healthcare & Life Sciences | `TextQLLabs/ontology-healthcare-starter` | ✅ validated | claims/clinical; ICD-10 / CCSR / CMS-HCC; PMPM, readmission, RAF, HEDIS |
| Financial Services (banking/payments/lending) | `TextQLLabs/ontology-finserv-starter` | 🟡 v1 | MCC / NAICS; deposits, NIM, delinquency, charge-offs, fraud |
| Insurance | — | ⚪ planned | branch of the finserv pattern (policy/claim/premium; loss & combined ratio) |
| Retail / CPG | — | ⚪ planned | |

## Enterprise functions — recipes + light templates
More company-specific, so build these with the recipes rather than a heavy prebuilt starter.

| Function | Build with | Notes |
|---|---|---|
| Finance | `recipe-03` (warehouse) + `recipe-06` (definitions: revenue, margin) | reconcile team-specific definitions |
| Logistics / Supply chain | `recipe-03` + `recipe-05` (workflows) | model the flow + the source data |
| Materials management | `recipe-03` + `recipe-06` | |
| Reporting / Dashboards | `recipe-09` (playbook/dashboard) | |
| HR / People | `recipe-06` + `recipe-08` (team area) | |

## Adding a domain
- **Vertical** with regulated/standard code sets and metrics → build a full starter (mirror the
  healthcare/finserv six-layer pattern), give it a `SKILL.md`, and add a row above.
- **Function** → usually recipes + a light template; add a row in the functions table.
- Keep the maturity calibrated: a starter ships as 🟡 v1 (structure complete) and becomes ✅ once
  it's been validated against a live connector (see each starter's `validation/`).
