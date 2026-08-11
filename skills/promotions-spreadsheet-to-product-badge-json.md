---
name: promotions-spreadsheet-to-product-badge-json
description: Transform Browse Path promotions spreadsheet data (CSV/XLSX export) into product-badge service JSON keyed by market and SKU. USE FOR coupon spreadsheet normalization, field mapping, date normalization, and deterministic JSON generation. DO NOT USE FOR unrelated data pipelines or analytics reporting.
---

# Promotions Spreadsheet to Product-Badge JSON

Use this skill to convert promotions rows from spreadsheet exports into the JSON format consumed by the product-badge service.

## Target Output Shape

There are two supported promotion types:

- `Regular` promotion
- `For Two` promotion

```json
{
  "US-AZ": {
    "Skus": {
      "115676175": {
        "QrCode": 2064,
        "OfferCode": "LTA_QUOTIENT_2026_06_3OFF_JACKDANIELS",
        "Description": "$3.00 Off Coupon",
        "Details": "Save $3 when you buy ONE (1) 1.75L bottle... VALID WHILE SUPPLIES LAST...",
        "StartDate": "2026-06-08T00:00:00Z",
        "EndDate": "2026-07-31T23:59:59Z"
      }
    }
  }
}
```

Promotion-type examples:

`Regular` promotion example:

```json
"115676175": {
  "QrCode": 2064,
  "OfferCode": "LTA_QUOTIENT_2026_06_3OFF_JACKDANIELS",
  "Description": "$3.00 Off Coupon",
  "Details": "Save $3 when you buy ONE (1) 1.75L bottle of any Jack Daniel's Tennessee Whiskey, Apple, Honey, Blackberry, or Fire. VALID WHILE SUPPLIES LAST. Must be of legal purchase age to participate. Must be redeemed at time of purchase. You pay any sales tax or deposit. Offer not valid for employees of Brown-Forman Beverages, affiliated companies and agencies, licensed distributors, retailers, and their immediate families and household members. Void where taxed, restricted, or prohibited by law. Please Drink Responsibly. Responsibility.org. Cannot be combined with any other Total Wine & More coupon or discount. &More Members Only.",
  "StartDate": "2026-06-08T00:00:00Z",
  "EndDate": "2026-07-31T23:59:59Z"
}
```

`For Two` promotion example:

```json
"4833750": {
  "QrCode": 2058,
  "OfferCode": "LTA_QUOTIENT_2026_06_2OFF_MARKWEST_NOBILO",
  "Description": "$2.00 Off Two",
  "Details": "SAVE $2.00 when you buy TWO (2) 750ml bottles of Mark West, Nobilo, or Franciscan VALID WHILE SUPPLIES LAST. OFFER VOID WHERE PROHIBITED BY LAW. Employees of any of the following are not eligible to participate: Franciscan, Mark West, Nobilo affiliated companies, state liquor commissions, and other alcoholic beverage-related government agencies; alcoholic beverage licensees; any household member or immediate family of any employee of one of these entities. Limit 1 rebate per household. No obligation arises from purchase of the designated product or from issuance of any rebate check. Rebate check must be presented for payment before the sponsoring company has any obligation to pay this rebate. Purchase, sale, or trade of rebate form, or UPC symbols is prohibited. Cannot be combined with any other Total Wine & More coupon or discount. &More Members Only.",
  "StartDate": "2026-06-01T00:00:00Z",
  "EndDate": "2026-07-31T23:59:59Z"
}
```

## Input Columns (Browse Path Spreadsheet)

Map from these source columns:

- `Offer QrId (PRD #)`
- `Locations`
- `Hybris Codes - JULY` (or month-specific equivalent)
- `Offer (Get from SharePoint)`
- `Subheader (Get from SharePoint)`
- `Offer Details / T&Cs (SharePoint)`
- `Start Date`
- `End Date`

Optional metadata columns (not emitted but useful for QA):

- `Top 30 Item for Marketing?`
- `QRID being reused? If yes, need to add "2" & submit JIRA`
- `Month / Round`
- `Funding`

## Field Mapping Rules

For each valid spreadsheet row:

1. `marketKey`
- Source: `Locations`
- Transform:
  - Split by comma if multiple values
  - Trim whitespace
  - Prefix each state with `US-`
- Example: `AZ` -> `US-AZ`

2. SKU list
- Source: `Hybris Codes - JULY`
- Transform:
  - Split on commas and newlines
  - Trim whitespace
  - Remove empty tokens
  - Keep as strings in JSON keys
  - De-duplicate within the row

3. `QrCode`
- Source: `Offer QrId (PRD #)`
- Transform:
  - Parse as integer
  - Skip row if missing or invalid

4. `OfferCode`
- Source: `Offer (Get from SharePoint)`
- Transform:
  - Preserve exact value (trim only)

5. `Description`
- Preferred derivation:
  - Extract first money token from `Subheader` or `Offer` (for example `$3`, `$3.00`, `$1.50`)
  - Normalize to two decimals
  - Compose as `$X.XX Off Coupon` for `Regular` promotions
- Promotion type detection:
  - Detect `For Two` when selected offer wording explicitly indicates a two-item requirement (for example, `buy TWO (2)`, `2 bottles`, `two items`)
  - Otherwise treat as `Regular`
- Quantity labeling rule:
  - `Regular` -> `$X.XX Off Coupon`
  - `For Two` -> `$X.XX Off Two`
- Fallback:
  - If amount cannot be extracted, use `Offer` text title-cased with minimal normalization

6. `Details`
- Compose from:
  - Primary promo sentence: `Subheader`
  - Terms copy: `Offer Details / T&Cs`
- Transform:
  - Join with a single space
  - Collapse internal whitespace/newlines to single spaces
  - Keep punctuation and legal text intact

7. `StartDate`
- Source: `Start Date` (`M/D/YYYY` or `MM/DD/YYYY`)
- Transform:
  - Convert to UTC day start: `YYYY-MM-DDT00:00:00Z`

8. `EndDate`
- Source: `End Date`
- Transform:
  - Convert to UTC day end: `YYYY-MM-DDT23:59:59Z`

## Record Expansion Logic

Each row can map to many SKUs and optionally many markets.

For every `(market, sku)` pair generated from the row, write:

```json
"<market>": {
  "Skus": {
    "<sku>": {
      "QrCode": <int>,
      "OfferCode": "<string>",
      "Description": "<string>",
      "Details": "<string>",
      "StartDate": "<ISO>",
      "EndDate": "<ISO>"
    }
  }
}
```

If the same SKU appears multiple times within the same market:

- Prefer the row with the latest `StartDate` when offer windows overlap.
- If windows are identical, prefer the row with the newest non-empty `OfferCode`.
- Emit a warning in validation output for manual review.

## Data Quality Checks

Run these checks before finalizing JSON:

1. Required values present for each emitted row:
- `QrCode`, `OfferCode`, at least one SKU, `Start Date`, `End Date`, `Locations`

2. Date sanity:
- `StartDate <= EndDate`

3. QR integrity:
- `QrCode` is numeric
- Flag rows marked as reused in spreadsheet metadata for review

4. SKU integrity:
- Only numeric SKU tokens
- No blank SKU keys

5. Text integrity:
- `Details` not empty
- No embedded hard line-break sequences after normalization

## Source-of-Truth Text Policy (No Silent Corrections)

When translating spreadsheet data, treat spreadsheet text as source-of-truth.

- Do not silently fix typos, grammar, capitalization, punctuation, brand spelling, or legal copy.
- Do not change offer wording to "improve" readability unless explicitly requested.
- Allowed automatic text edits are limited to whitespace normalization required by the JSON format (trim, collapse repeated spaces/newlines).
- If a suspected typo or mismatch is found between spreadsheet and expected output, pause and ask the user for confirmation before changing content.
- If user-approved corrections are applied, report them explicitly as a change list in output notes.

## Deterministic Output Rules

To keep generated files stable across runs:

- Sort market keys alphabetically
- Sort SKU keys numerically-as-strings (stable lexicographic if mixed)
- Use two-space indentation
- Use UTF-8 JSON without trailing commas

## Suggested Implementation Flow

1. Parse spreadsheet export (CSV preferred for deterministic parsing).
2. Normalize headers to exact names.
3. Iterate rows and skip fully blank records.
4. Build promo payload per row using mapping rules.
5. Expand to all SKUs and locations.
6. Apply conflict resolution.
7. Run quality checks and collect warnings.
8. Emit final JSON file.

## Validation Against Known Good Output

When an example final JSON is available:

- Compare key counts per market
- Compare SKU set equality
- Spot-check random SKU payloads (`QrCode`, `OfferCode`, dates)
- Diff normalized `Details` text for punctuation/whitespace drift

## Example Prompt to Use This Skill

"Translate this Browse Path coupon spreadsheet into product-badge JSON using the promotions-spreadsheet-to-product-badge-json skill. Validate date formatting, split all Hybris codes into SKU keys, and emit grouped output by market."