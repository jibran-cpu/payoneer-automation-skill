# Process Stage Reference

## Procedure Map

| Procedure | Raw Fields Used | Output / Result |
|---|---|---|
| Raw data review and validation | `Transaction Date`, `Transaction ID`, `Description`, `Debit Amount`, `Credit Amount`, `Currency`, `Status`, `Source`, `Target`, `Running Balance` | Validated rows, exception flags, exclusion reason where necessary |
| Month identification | `Transaction Date` | `Reporting Period`, `Reporting Month`, `Reporting Year` |
| Transaction grouping and Head of Account mapping | `Target`, `Source`, plus `Description`, `Store Name`, `Additional Description` | `Head of Account`, `Vendor Name`, classification rule |
| Currency-wise segregation | `Currency` | USD and EUR working streams/tabs |
| EUR to USD translation | `Currency`, `Debit Amount`, `Credit Amount`, `Transfer Amount` | USD-equivalent amount using fixed rate `1.18` |
| Amount derivation and sign convention | `Debit Amount`, `Credit Amount` | Net expense amount: debit positive, credit negative |

## Standardization

Preserve all raw fields. Add standardized fields separately:

```text
Source File
Raw Row Number
Standard Transaction Date
Standard Transaction Time
Credit Amount Numeric
Debit Amount Numeric
Transfer Amount Numeric
Running Balance Numeric
Clean Description
Clean Source
Clean Target
Account Currency
```

## Sign Convention

Use this logic:

```text
If Debit Amount exists and Credit Amount is blank:
  Net Amount = Debit Amount

If Credit Amount exists and Debit Amount is blank:
  Net Amount = Credit Amount * -1

If both Debit Amount and Credit Amount are blank:
  Flag exception

If both Debit Amount and Credit Amount are populated:
  Flag exception
```

## EUR Translation

Use USD as reporting currency.

```text
If Currency = EUR:
  USD Equivalent Amount = Net Amount * 1.18

If Currency = USD:
  USD Equivalent Amount = Net Amount
```

Create:

```text
Original Currency
Original Amount
FX Rate
USD Equivalent Amount
```

Use `1.18` as the fixed EUR-to-USD rate unless the user provides another rate.

## Transaction Type

Suggested rules:

```text
Description contains "Card charge" -> Card Charge
Description contains "Payment from" -> Receipt
Description contains "Payment to" -> Payment / Withdrawal
Description contains "Transfer between balances" -> Balance Transfer
Transfer Amount populated -> Transfer / FX Movement
Otherwise -> Review Required
```

## Head of Account Mapping

Primary fields:

```text
Target
Source
```

Secondary fields:

```text
Description
Store Name
Additional Description
```

Normalize common vendor variants before mapping:

```text
OPENAI *CHATGPT SUBSCR -> OPENAI
Card charge (OPENAI) -> OPENAI
CURSOR, AI POWERED IDE -> CURSOR, AI POWERED
APPLEADS/APPLE164 -> APPLE ADS
Amazon web services -> Amazon Web service.
Google CLOUD W3MWBF -> Google Cloud
```

Typical sections:

```text
Marketing and Campaign
IT tools and software
Indirect- Dues & Subscription
Prepayments
Transfers
Receipts
Bank Withdrawals
Unclassified
```

Create:

```text
Vendor Name
Normalized Source
Normalized Target
Head of Account
Section
Subcategory
Classification Rule Used
Classification Confidence
Review Required
```

## Exceptions

If a row is not confidently usable, flag it. If exclusion is necessary, make it visible.

Exception fields:

```text
Exception Flag
Issue Type
Issue Details
Include / Exclude Status
Exclusion Reason
Review Status
```

Common issues:

```text
Missing required field
Duplicate Transaction ID
Invalid date
Invalid amount
Status not Completed
Both debit and credit blank
Both debit and credit populated
Missing Source
Missing Target
Unmapped Head of Account
Currency mismatch
FX translation issue
Running Balance inconsistency
```
