# Skills Catalog

## Global Conventions

- Preserve raw Payoneer CSV files unchanged.
- Use `Transaction ID` as the audit and duplicate-control key.
- Use debit-positive and credit-negative sign convention.
- Translate EUR to USD at fixed rate `1.18` unless the user provides another rate.
- Use validation gates before generating final outputs.
- Make final workbooks/PDFs self-contained and formula-error-free.

## payoneer-automation

Description: Defines the 9D Payoneer automation workflow from USD/EUR Payoneer raw CSVs through validation, processing, EUR-to-USD translation, Head of Account mapping, working schedules, `Output.pdf`-style summaries, and exception reports.

Trigger phrases:

- "Build the Payoneer automation"
- "Review Payoneer raw CSVs"
- "Create Payoneer SOP/process flow"
- "Generate Payoneer working file and output summary"

Inputs:

- Payoneer USD raw CSV
- Payoneer EUR raw CSV
- `process flow.xlsx`
- `Working file.xlsx`
- `Output.pdf`
- Optional vendor/category/FX/manual-lines mappings

Outputs:

- Auditable automation plan or SOP
- Standardized processed transaction schema
- Currency-wise working schedules
- Period-based Head of Account summary
- Exception report

Upstream dependencies:

- not specified

Downstream consumers:

- `xlsx`
- `spreadsheets`
- `pdf`
- `skill-orchestration-planner`

Key files:

- `SKILL.md`
- `references/raw-data.md`
- `references/process-stage.md`
- `references/outputs.md`
- `references/orchestration.md`

Notes:

- Use this skill for business rules and handoff definitions. Use spreadsheet/xlsx capabilities to create or verify workbook artifacts and PDF capabilities to inspect or match the final PDF-style output.

## xlsx / spreadsheets

Description: Create, edit, analyze, render, and verify spreadsheet workbooks used by the Payoneer automation.

Trigger phrases:

- "Create the working file"
- "Build the output workbook"
- "Validate formulas"
- "Update Excel schedules"

Inputs:

- Processed Payoneer transaction table
- Mapping/config tables
- Existing workbook templates

Outputs:

- `Working file.xlsx`
- Summary workbook
- Formula and layout validation results

Upstream dependencies:

- `payoneer-automation`

Downstream consumers:

- `pdf`

Key files:

- not specified

Notes:

- Use when artifact creation or workbook verification is requested.

## pdf

Description: Read, extract, compare, or create PDF outputs for the Payoneer final summary.

Trigger phrases:

- "Check Output.pdf"
- "Match the PDF layout"
- "Export the final summary to PDF"
- "Extract the PDF table"

Inputs:

- `Output.pdf`
- Rendered or exported final summary

Outputs:

- PDF extraction notes
- Final PDF checks
- PDF deliverable where requested

Upstream dependencies:

- `payoneer-automation`
- `xlsx` / `spreadsheets`

Downstream consumers:

- not specified

Key files:

- not specified

Notes:

- Treat PDF as layout/reference evidence, not the primary calculation source.

## skill-orchestration-planner

Description: Maintain skill inventory and plan multi-skill workflow sequencing, dependencies, validation gates, and handoffs.

Trigger phrases:

- "Update the skill catalog"
- "Plan skill orchestration"
- "Create an orchestration plan"
- "Define pipeline handoffs"

Inputs:

- Skill folders and `SKILL.md` files
- `skills-catalog.md`
- Existing process notes

Outputs:

- Updated `skills-catalog.md`
- `orchestration_plan.md`

Upstream dependencies:

- not specified

Downstream consumers:

- `payoneer-automation`
- `xlsx`
- `spreadsheets`
- `pdf`

Key files:

- `skills-catalog.md`
- `references/orchestration.md`

Notes:

- Use for planning and coordination, not for replacing each skill's own runtime instructions.
