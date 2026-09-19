# 2027 Budget Web App — Discovery Notes

Working notes captured during project kickoff. This file is the running
record of requirements as they are discussed. It will be reorganized into
a proper spec once discovery is complete.

## How this discovery is being run

- Ken walks through his thoughts in segments, one topic per message, to
  avoid timing or connectivity issues and to allow time to think.
- Claude records each segment here verbatim in substance, commits it, and
  replies only with a go-ahead. No questions are asked mid-walkthrough.
- Questions that come up while recording are parked in the
  "Parked questions" section at the bottom, grouped by category.
- Once Ken says he has finished noting everything, Claude raises the
  parked questions one category at a time, and each group is resolved
  before moving to the next.

## Segment log

| # | Topic | Status |
|---|-------|--------|
| 1 | Background, move off Airtable, source material | Recorded |
| 2 | Units as the basis of revenue; product structure | Recorded |
| 3 | Company structure: departments and product lines; GL accounts | Recorded |

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

## Segment 3: Company structure — departments and product lines

### Two organizing dimensions

The company's accounting is organized along two axes:

1. **Department** — how expenses are classified.
2. **Product line** — how sales and product-related activity are
   classified.

### Revenue always sits in the Revenue department

- Revenue is booked to a single Revenue department, effectively an
  "empty" department used only for revenue.
- Expenses and product sales are identified by department, but revenue
  is never split across operating departments.
- Example: if Marketing happened to sell consulting services, it would
  not be classified as Marketing revenue. It would be classified by its
  product line (Consulting Services) with the department set to Revenue.
- Rule for the app: **everything on the revenue side goes through the
  Revenue department.** Product line is the meaningful dimension for
  revenue; department is fixed.

### Product lines (as listed so far)

Active lines:

| Product line | Notes |
|---|---|
| Cyclone Rake | Dual-pin hitch system. The flagship. Most units, most accessories, most spare parts, and therefore the most GL lines (close to 100). |
| Cyclone Rake Single | Single-pin hitch system. Very similar to the Cyclone Rake. Some cost differences and a slight revenue uplift because single carries a small premium to the customer. |
| Cyclone Nut Rake | Distinct product, previously noted in segment 2. |
| Medical | Named as a product line. Details not yet discussed. |
| EarthBox | Named as a product line. Details not yet discussed. |
| Hose business | Named as a product line. Ken will come back to this. |
| MDA (Mower Deck Adapter) | Aftermarket mower deck adapter business. Sold to customers and non-customers. |
| Recon Power | Returned product that has been refurbished and is resold as near-new but not brand new. |

Legacy lines, no longer sold but still supported in the field:

| Product line | Notes |
|---|---|
| Super Hauler | Discontinued. Units remain in the field. Activity is aftermarket parts and accessories, for example engine maintenance kits (oil, air filter, and similar). |
| Small Property Solution | Discontinued. Same situation as Super Hauler: parts and accessories for units in the field. |

Dormant or near-dormant lines:

- A "fall cleanup" or "leaf fall" line (exact name uncertain) that was
  set up but never really developed.
- Possibly one or two others Ken could not recall. These have either no
  activity or only occasional activity.

### GL accounts per product line

- Each product line has anywhere from a couple of GL accounts up to
  roughly 100 lines.
- Some GL accounts are used by multiple product lines, but the activity
  within them is specific to the product line.
- Cyclone Rake has the most lines because it has the most units, the
  most accessories driven by those units, spare parts, and other
  related items.

### Implications for the data model

- The chart of accounts is a grid: **product line × GL account**, not a
  flat account list. The same GL account can appear under several
  product lines with separate budget values.
- Product lines need a status: active, legacy (parts and accessories
  only), dormant. The report may want to show or suppress lines
  differently based on status.
- Within a product line, lines fall into categories such as units,
  accessories, spare parts, and maintenance kits. Some of these are
  unit-driven and some are dollar-only. The model must allow both on
  the same product line.
- The Cyclone Rake and Cyclone Rake Single share a structure but have
  different costs and prices. The catalog should let them share line
  definitions while holding separate values.
- Department is a required dimension for expenses and is constant
  (Revenue) for the revenue side. The schema should carry department on
  every line so that expenses can use it later without a redesign.

## Parked questions

Held until Ken finishes his walkthrough. Grouped by category so they can
be worked through one group at a time.

### Products and revenue model

- Full list of product lines: confirm the ones Ken could not recall,
  and the exact name of the "fall cleanup / leaf fall" line.
- Medical and EarthBox: what are these lines, and are they unit-driven?
- Hose business: Ken said he would come back to this.
- Recon Power: is it its own product line on the report, or a
  sub-line under the parent product (Cyclone Rake, etc.)?
- Legacy lines (Super Hauler, Small Property Solution): shown on the
  report as their own lines, or folded into a parts/accessories group?
- Within a product line, which GL lines are unit-driven versus
  dollar-only? Is there a consistent pattern (units, accessories,
  spare parts, kits)?
- Which GL accounts are shared across multiple product lines?

- Bundle mechanics: modifier on a model sale, or separate SKUs? Does
  every model offer both bundles?
- Units budgeted by month, or by season with a monthly spread applied?
- One annual price per model, or per-month pricing?
- Other revenue streams beyond rakes, nut rake, and adapter (parts,
  accessories, shipping, service)?
- Does sales channel (direct, dealer, online) matter to the report?

### Source data and reporting

- Airtable table exports and the Excel final report still to be received.
- Where do actuals come from for prior-year comparison?

### People and workflow

- Who enters, who reviews, who approves?
- How many rounds of review, and is there a lock or sign-off step?

### Platform

- Hosting preference and tolerance for a monthly cloud cost.
- Microsoft 365 sign-in requirement?
- Target date for opening 2027 budget entry.
