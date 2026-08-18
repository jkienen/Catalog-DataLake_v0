# Data Lake Documentation Template (Legacy)

> A Data Lake concept in a single spreadsheet — every source, rule and automation mapped in one consistent, auditable place.

> 🔻 **This is the legacy version.** The active catalog is [`Catalog-DataLake_v1`](https://github.com/jkienen/Catalog-DataLake_v1):
>
> - one YAML file per entry, not one row in a shared sheet
> - filled by an AI skill reading the code, not by hand
> - Audit promoted to its own layer, and the whole thing rendered as a browsable graph

<p align="center">
  <img src="Files/architecture-concept.png" alt="Data Lake concept diagram: EDR, SIEM and PAM sources feed the Raw layer, transformed into Silver by business rules, branching into Gold indicators that answer business questions and into Automations that apply fixes back to the sources.">
</p>

---

## What the Workbook Got Right, and Where It Stopped Scaling

The workbook centralizes the knowledge a growing Data Lake usually scatters across people, scripts and chat history: where each source comes from, how it is transformed, what each indicator means, which automations act on it. One row per source, rule or automation, the same fields every time, organized around the medallion architecture (Raw → Silver → Gold) plus an Automations layer — no tooling required, and it runs anywhere a spreadsheet does.

It stopped scaling for three reasons. **Documenting one automation took a full day** — an analyst reading the code, then transcribing every endpoint, filter, transformation and KPI by hand. **The sheets grew into the thousands of rows**, and finding one entry, or spotting a duplicate, meant scrolling and filtering across sheets never built to hold that much. **Consistency depended on discipline, not structure** — nothing stopped two rows from describing the same rule differently, or a field from being skipped, because a cell enforces no shape.

[`Catalog-DataLake_v1`](https://github.com/jkienen/Catalog-DataLake_v1) keeps every idea this version got right and changes how entries get produced and read — see its README for what that looks like.

---

<details>
<summary><strong>How It Works</strong> (click to expand)</summary>

The documentation mirrors the lifecycle of the data across four layers:

<table width="100%">
<thead>
<tr><th>Layer<div><img width="200" height="1" alt=""></div></th><th>What it holds<div><img width="700" height="1" alt=""></div></th><th>Documented in<div><img width="300" height="1" alt=""></div></th></tr>
</thead>
<tbody>
<tr><td><strong>Raw</strong></td><td>Data ingested exactly as the source delivers it, with no transformation.</td><td><code>Endpoints (Raw)</code></td></tr>
<tr><td><strong>Silver</strong></td><td>Cleaned, cross-referenced, and structured data, ready for analysis.</td><td><code>Business Rules (Silver &amp; Gold)</code></td></tr>
<tr><td><strong>Gold</strong></td><td>Business indicators / KPIs derived from the Silver layer.</td><td><code>Business Rules (Silver &amp; Gold)</code></td></tr>
<tr><td><strong>Automations</strong></td><td>Automated actions driven by the curated data.</td><td><code>Automations</code></td></tr>
</tbody>
</table>

Each layer feeds the next: Raw is the input to the Silver rules, the Silver output is consolidated into Gold indicators, and Automations consume the curated datasets to act.

<details>
<summary><strong>Detailed architecture</strong> (click to expand)</summary>

The diagram above shows the concept; this is the same flow at dataset level, with sources, endpoints, Silver/Audit/Gold groupings and automations all named individually.

<p align="center">
  <img src="Files/architecture-full.png" alt="Detailed Data Lake architecture: every Raw endpoint, Silver/Audit/Gold dataset and Automation routine, grouped by source and project.">
</p>

</details>

</details>

<details>
<summary><strong>Workbook Structure</strong> (click to expand)</summary>

The template has three sheets. Fill **one row per item**.

### 1. `Endpoints (Raw)`: the source catalog

Documents every source endpoint feeding the Raw layer.

<details>
<summary><strong>Sheet preview</strong> (click to expand)</summary>

<p align="center">
  <img src="Files/Level-l.png" alt="Endpoints (Raw) sheet: CrowdStrike platform with USB Query and USB Policies endpoints, status, and extraction ownership notes.">
</p>

</details>

<table width="100%">
<thead>
<tr><th>Field<div><img width="350" height="1" alt=""></div></th><th>Purpose<div><img width="850" height="1" alt=""></div></th></tr>
</thead>
<tbody>
<tr><td><code>Platform</code></td><td>The source system the data comes from</td></tr>
<tr><td><code>Endpoint</code></td><td>The specific API/endpoint(s) used for extraction</td></tr>
<tr><td><code>Status</code></td><td>Lifecycle of the integration (dropdown)</td></tr>
<tr><td><code>Description</code></td><td>What the endpoint returns, in plain language</td></tr>
<tr><td><code>Load Type</code></td><td><code>Full</code> (full snapshot) or <code>Incremental</code> (only new data)</td></tr>
<tr><td><code>Frequency</code></td><td>How often the extraction runs</td></tr>
<tr><td><code>Volume per Frequency</code></td><td>Approximate data volume per run</td></tr>
<tr><td><code>Execution Schedule</code></td><td>When the routine runs</td></tr>
<tr><td><code>Routine Name</code></td><td>Identifier of the ingestion routine</td></tr>
<tr><td><code>Extraction</code></td><td>How the data is extracted and who is responsible</td></tr>
<tr><td><code>Filters</code></td><td>Conditions / query parameters applied at the source</td></tr>
<tr><td><code>Storage (Raw)</code></td><td>Where and how the raw output is stored (path convention)</td></tr>
<tr><td><code>Sample (Raw)</code></td><td>A representative sample of the stored record</td></tr>
<tr><td><code>Last Review Date</code></td><td>When the entry was last reviewed</td></tr>
<tr><td><code>Owner</code></td><td>Person accountable for the integration</td></tr>
<tr><td><code>Base Project (Github)</code></td><td>Repository hosting the routine code</td></tr>
</tbody>
</table>

### 2. `Business Rules (Silver & Gold)`: transformations and indicators

Documents how Raw data becomes structured Silver datasets, and the Gold indicators derived from them.

<details>
<summary><strong>Sheet preview</strong> (click to expand)</summary>

<p align="center">
  <img src="Files/Level-ll.png" alt="Business Rules (Silver & Gold) sheet: Vulnerability Management context with status, review date, and the four Gold indicators (fixed vulns, MTTR by severity) with calculation and rationale.">
</p>

</details>

<table width="100%">
<thead>
<tr><th>Field<div><img width="350" height="1" alt=""></div></th><th>Purpose<div><img width="850" height="1" alt=""></div></th></tr>
</thead>
<tbody>
<tr><td><code>Context/Project</code></td><td>The business context / dataset being produced</td></tr>
<tr><td><code>Status</code></td><td>Lifecycle of the rule (dropdown)</td></tr>
<tr><td><code>Sources (Raw)</code></td><td>Raw inputs the rule reads from (with paths)</td></tr>
<tr><td><code>Sources (Silver)</code></td><td>Silver inputs cross-referenced by the rule (with paths)</td></tr>
<tr><td><code>Execution Schedule</code></td><td>When the processing runs</td></tr>
<tr><td><code>Routine Name</code></td><td>Identifier of the processing routine</td></tr>
<tr><td><code>Filters</code></td><td>Conditions applied during processing</td></tr>
<tr><td><code>Processing Pipeline</code></td><td>Step-by-step description of the transformation logic</td></tr>
<tr><td><code>Data Structuring (Silver)</code></td><td>The columns/structure of the resulting Silver dataset</td></tr>
<tr><td><code>Generated Audits</code></td><td>Secondary datasets produced for tracking/exceptions</td></tr>
<tr><td><code>Storage (Silver)</code></td><td>Where and how the Silver output is stored (path convention)</td></tr>
<tr><td><code>Output (Silver)</code></td><td>A representative sample of the structured record</td></tr>
<tr><td><code>Owner</code></td><td>Person accountable for the rule</td></tr>
<tr><td><code>Base Project (Github)</code></td><td>Repository hosting the routine code</td></tr>
<tr><td><code>Last Review Date</code></td><td>When the entry was last reviewed</td></tr>
<tr><td><code>Indicators (Gold)</code></td><td>The indicators/KPIs derived, with calculation, filters, and rationale</td></tr>
</tbody>
</table>

### 3. `Automations`: automated actions

Documents automations that consume the curated data to act on the environment.

<details>
<summary><strong>Sheet preview</strong> (click to expand)</summary>

<p align="center">
  <img src="Files/Level-lll.png" alt="Automations sheet: Tag Synchronization, Unwatched Assets Cleanup, and Removal of Unused Exceptions routines with status, sources, frequency, schedule, and filters.">
</p>

</details>

<table width="100%">
<thead>
<tr><th>Field<div><img width="350" height="1" alt=""></div></th><th>Purpose<div><img width="850" height="1" alt=""></div></th></tr>
</thead>
<tbody>
<tr><td><code>Routine</code></td><td>What the automation does, in plain language</td></tr>
<tr><td><code>Status</code></td><td>Whether the automation is enabled (dropdown)</td></tr>
<tr><td><code>Sources (Raw)</code> / <code>Sources (Silver)</code></td><td>Datasets the automation reads from (with paths)</td></tr>
<tr><td><code>Frequency</code></td><td>How often it runs</td></tr>
<tr><td><code>Execution Schedule</code></td><td>When it runs</td></tr>
<tr><td><code>Filters</code></td><td>Conditions that select what to act on</td></tr>
<tr><td><code>Processing Pipeline</code></td><td>Step-by-step description of the action logic</td></tr>
<tr><td><code>SOAR Routine Name</code></td><td>Identifier of the automation in the orchestration tool</td></tr>
<tr><td><code>Last Review Date</code></td><td>When the entry was last reviewed</td></tr>
<tr><td><code>Base Project (Github)</code></td><td>Repository hosting the automation code</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>How to Use</strong> (click to expand)</summary>

> 📝 **The content currently in the workbook is illustrative only.** It exists to show *how* to fill each field. Replace every example row with your own sources, rules, and indicators.

1. Pick the sheet for what you are documenting (a source, a rule, or an automation).
2. Add **one row** and fill every applicable field, using the dropdowns for `Status`.
3. Follow the **path and naming conventions** above so the entry stays consistent with the rest.
4. Record an `Owner` and a `Last Review Date` so the entry has clear accountability.
5. Keep a representative **sample** in the `Sample` / `Output` field; it is the fastest way for a reader to understand the data shape.

> ✅ Treat the workbook as living documentation: review entries periodically and keep `Status` and `Last Review Date` up to date.

</details>

---

## License

Provided as-is for reference and reuse. Adapt it freely to your environment.
