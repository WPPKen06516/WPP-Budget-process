# 2027 Budget Web App — Discovery Notes

Working notes captured during project kickoff. This file is the running
record of requirements as they are discussed. It will be reorganized into
a proper spec once discovery is complete.

## Background

- 2025: the 2026 budget was built in Airtable.
- 2026: the 2027 budget needs a new path. This repo is the new web app.
- Source material to be provided by Ken:
  - The Airtable tables from the 2026 budget process.
  - The Excel workbook used to produce the final budget report. The
    Airtable tables did not present the numbers the way the business
    wanted, so the Excel report is the presentation of record.
- The final report format must be preserved. The new app should produce
  the same presentation, not force a new one.

## Core requirement: units, not just dollars

Revenue must be tracked at two levels:

1. Total revenue dollars.
2. The unit quantities that drive those dollars, by product.

This is the main thing Airtable did not handle well and the primary
reason for a purpose-built app.

## Product structure (as described so far)

### Rakes (main product line)

- Seven rake models currently sold.
- Two bundle options that apply to rake sales.
- Open question: are bundles a modifier on a model sale, or sold as their
  own SKU? Does each model have both bundle options available?

### Nut rake

- A distinct product, separate from the seven main rake models.

### Mower deck adapter

- Aftermarket adapter sold to both existing customers and non-customers.
- Helps a buyer attach whatever unit they own, typically for leaf cleanup.
- Sold standalone, so it has its own unit and revenue line independent
  of rake sales.

## Implications for the data model

- Revenue lines need a `units` and a `unit_price` (or `average_selling
  price`) alongside `dollars`, with dollars derivable from units × price
  where that relationship holds.
- A product catalog table is required: model, product family (rake, nut
  rake, adapter), bundle options, active flag, effective dates.
- Budget by product by period (month), with roll-ups to product family
  and total revenue.
- The product list will change year to year, so products must be
  versioned by fiscal year rather than hard-coded.

## Open questions

- Bundle mechanics (see above).
- Do units get budgeted by month or by season, given leaf cleanup is
  seasonal?
- Are there other revenue streams beyond rakes, nut rake, and adapter
  (parts, accessories, service, shipping)?
- Is there a channel dimension (direct, dealer, online) that matters for
  the report?
- Where do actuals come from for prior-year comparison?
- Who enters, who reviews, who approves?
