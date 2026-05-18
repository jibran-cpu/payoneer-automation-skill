# Raw Data Reference

## Source

Source system: Payoneer  
File type: CSV  
Expected files: separate Payoneer raw exports for USD and EUR balances.

The file name may vary. Use headers and the `Currency` column as source of truth.

## Required Schema

Valid raw Payoneer CSV files must contain these headers:

```text
Transaction Date
Transaction Time
Time Zone
Transaction ID
Description
Credit Amount
Debit Amount
Currency
Transfer Amount
Transfer Amount Currency
Status
Running Balance
Additional Description
Store Name
Source
Target
Reference ID
```

## Field Purposes

- `Transaction Date`: accounting date; drives reporting month and YTD allocation.
- `Transaction Time`: audit sequencing within the same day.
- `Time Zone`: timing alignment, usually `UTC`.
- `Transaction ID`: unique Payoneer reference; primary duplicate and audit key.
- `Description`: transaction narration; supports transaction type and expense classification.
- `Credit Amount`: receipts, refunds, or incoming funds.
- `Debit Amount`: expenses, payments, withdrawals, or outgoing transfers.
- `Currency`: balance/transaction currency, usually `USD` or `EUR`.
- `Transfer Amount`: settlement/transfer value for balance movement or FX context.
- `Transfer Amount Currency`: currency of `Transfer Amount`; supports FX validation.
- `Status`: transaction status; normally process `Completed` rows.
- `Running Balance`: wallet balance after transaction; supports reconciliation control.
- `Additional Description`: optional note or invoice reference.
- `Store Name`: optional merchant field, often blank.
- `Source`: originating wallet/card/employee/payer; supports source attribution and grouping.
- `Target`: merchant/beneficiary/destination; primary field for Head of Account mapping.
- `Reference ID`: optional external/internal reference for audit linkage.

## Field Groups

```text
Transaction identity:
Transaction ID, Transaction Date, Transaction Time, Time Zone, Reference ID

Money:
Credit Amount, Debit Amount, Currency, Transfer Amount, Transfer Amount Currency, Running Balance

Classification:
Description, Source, Target, Store Name, Additional Description

Control:
Status
```

## Validation Expectations

Minimum fields for processing:

```text
Transaction Date
Transaction ID
Description
Credit Amount
Debit Amount
Currency
Status
Source
Target
```

Strongly recommended:

```text
Transaction Time
Time Zone
Running Balance
Transfer Amount
Transfer Amount Currency
Additional Description
Reference ID
```

Optional:

```text
Store Name
```

## Tolerance Rules

Tolerate:

```text
Different file names
Blank Store Name
Blank Additional Description
Blank Reference ID
Blank Transfer Amount for non-transfer rows
Blank Transfer Amount Currency for non-transfer rows
Minor text spacing differences
Different transaction order
Same Transaction Date and Transaction Time on multiple rows
```

Highlight:

```text
Missing required headers
Unexpected extra headers
Blank Transaction ID
Duplicate Transaction ID
Invalid Transaction Date
Non-numeric amount fields
Both Credit Amount and Debit Amount blank
Both Credit Amount and Debit Amount populated
Blank Currency
Currency mismatch against expected file/account currency
Status other than Completed
Blank Source or Target
Transfer Amount populated without Transfer Amount Currency
Transfer Amount Currency populated without Transfer Amount
Running Balance sequence inconsistency
```
