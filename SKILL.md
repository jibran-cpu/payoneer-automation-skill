---
name: payoneer-automation
description: Build, review, document, or implement the 9D Payoneer automation workflow for Payoneer USD/EUR CSV exports. Use when working with Payoneer raw data, SOPs, process flows, validation controls, vendor/head-of-account mapping, EUR-to-USD translation, working schedules, Output.pdf-style monthly summaries, or exception reports for campaign/subscription/IT-tools reporting.
---

# Payoneer Automation

## Overview

Use this skill to turn Payoneer USD and EUR raw CSV exports into an auditable automation plan, SOP, or implementation. Preserve raw data, validate aggressively, process into controlled working schedules, translate EUR to USD at the configured fixed rate, and produce a period-based Head of Account summary plus exceptions.

When the user asks for end-to-end implementation, combine this skill with spreadsheet/xlsx capabilities for workbook creation, PDF capabilities for reading or matching `Output.pdf`, and skill-orchestration-planner conventions for pipeline handoffs and validation gates.

## Core Workflow

Follow this order:

1. Inspect raw CSV files and confirm they use the standard Payoneer schema.
2. Validate required fields, duplicate controls, dates, amounts, status, currency, source, target, and running balance availability.
3. Standardize dates, times, numeric amounts, currency codes, source, target, and descriptions without overwriting raw values.
4. Assign reporting period from `Transaction Date`.
5. Derive transaction amount using debit-positive and credit-negative sign convention.
6. Segregate rows by `Currency` into USD and EUR working streams.
7. Translate EUR to USD using fixed FX rate `1.18`, unless the user gives another rate.
8. Group and classify transactions using `Target` and `Source`, with `Description`, `Store Name`, and `Additional Description` as secondary evidence.
9. Generate currency-wise working schedules.
10. Generate the final period-based summary in the `Output.pdf` style.
11. Produce an exception report for missing, invalid, duplicate, excluded, or unclassified rows.
12. Run quality checks: no silent exclusions, no formula errors, no unintended external links, and totals reconcile.

## Orchestration Pattern

Use a validation-gated linear pipeline:

```text
Raw CSV intake -> Validate -> Standardize -> Process/translate -> Classify -> Working schedules -> Output summary -> Exception/quality report
```

Block downstream output generation if file-level validation fails. Continue row-level processing only when exceptions are captured with `Include / Exclude Status` and `Exclusion Reason`.

For multi-skill work:

```text
payoneer-automation defines business rules and handoffs
xlsx/spreadsheets creates and verifies Excel workbooks
pdf reads/matches Output.pdf or renders final PDF
skill-orchestration-planner documents skill inventories and pipeline plans
```

## Reference Files

Load only the references needed for the current task:

- `references/raw-data.md`: required raw files, Payoneer headers, field purposes, required/optional fields, and validation tolerances.
- `references/process-stage.md`: procedures, field mapping, EUR translation, sign convention, grouping, classification, and exception handling.
- `references/outputs.md`: working file, final `Output.pdf`-style summary, section layout, period columns, and output quality controls.
- `references/orchestration.md`: skill dependencies, handoffs, validation gates, failure modes, and suggested multi-skill execution graph.
- `skills-catalog.md`: human-maintained inventory entry for this Payoneer workflow and adjacent skills.

## Important Rules

- Do not alter raw CSV files.
- Keep original raw fields and create separate standardized/processed fields.
- Use `Transaction ID` as the audit key and duplicate-control key.
- Treat `Debit Amount` as positive expense and `Credit Amount` as negative.
- Convert EUR values to USD with fixed rate `1.18` unless instructed otherwise.
- Use period-driven output columns. Do not force Jan-Dec unless the user asks for an annual template.
- Flag uncertainty instead of forcing classification.
- If excluding a row, record `Include / Exclude Status` and `Exclusion Reason`.
- Make final outputs self-contained. Do not rely on broken external workbook links.
