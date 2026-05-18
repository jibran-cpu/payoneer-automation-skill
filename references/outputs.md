# Output Reference

## Output Package

Produce these outputs when implementing the workflow:

```text
Working file.xlsx
Output.pdf or Output.xlsx/PDF export matching the Output.pdf layout
Exception Report
```

The final management deliverable is the `Output.pdf`-style monthly Head of Account summary.

## Working File

Expected sheets:

```text
Payoneer_USD
Payoneer_EURO
Exception Report
```

Purpose:

```text
Currency-wise classified schedules
Bridge from raw Payoneer transactions to final summary
Audit trail for Head of Account, month, source, and totals
```

Recommended working columns:

```text
Head of Accounts
<Month-YYYY> Amount
<Month-YYYY> Source
...
Total
```

If matching the legacy workbook is required, source columns may appear beside selected months. Prefer a consistent amount/source pair per month for automated builds.

EUR working values must either:

```text
Show USD-equivalent amounts with original EUR retained in support fields
or show original EUR, FX Rate, and USD Equivalent Amount explicitly
```

## Final Output Summary

The final output is period-driven, not automatically Jan-Dec.

For the current reference PDF, columns are:

```text
Head of Accounts
Jan-2026
Feb-2026
Mar-2026
Total
```

If later periods are present, include only the selected reporting months unless the user requests an annual template.

Required title area:

```text
9D TECHNOLOGIES LLC
Campaign and Subscription
```

Core output fields:

```text
Head of Account
Year-to-date monthly expense columns
Total
```

## Final Output Sections

Use these sections unless the user instructs otherwise:

```text
Marketing and Campaign
IT tools and software
Indirect- Dues & Subscription
Prepayments
TOTAL IT TOOL AND SUBSCRIPTION
Total Payoneer payout (in a month)
Other subscription / non-Payoneer lines
GRAND TOTAL
```

The reference PDF includes other subscription/non-Payoneer rows:

```text
Sunil _US Subscription(DIB AED)
Rizwan Malik HBL local card(9D SMC)
Hazel Mobile
9D SMC
Darwin-sensor tower
Helio soft-google workspace
ILK-google workspace
```

Do not treat `As per Joiin`, `Difference`, or `Third party` as mandatory final-output rows unless the user confirms they are required; they appeared in a corrupted Excel version, not the reliable PDF view.

## Summary Logic

For each Head of Account and reporting month:

```text
Output Amount = Sum of USD-equivalent processed amounts by Head of Account and period
```

Section totals:

```text
Total row = sum of rows within that section for each month
```

Grand total:

```text
GRAND TOTAL = TOTAL IT TOOL AND SUBSCRIPTION + other subscription/non-Payoneer rows
```

## Quality Controls

Check before finalizing:

```text
All raw files imported
All expected headers found
All valid transactions processed
Excluded rows listed in Exception Report
USD and EUR segregated correctly
EUR translated to USD at 1.18, unless overridden
Debit amounts treated as positive
Credit amounts treated as negative
Head of Account assigned or row flagged
Monthly columns populated correctly
Output totals agree to monthly columns
Grand Total calculated
No #REF!, #VALUE!, #DIV/0!, #NAME?, or #N/A errors
No unintended external workbook links
Output layout matches the PDF-style final view
```
