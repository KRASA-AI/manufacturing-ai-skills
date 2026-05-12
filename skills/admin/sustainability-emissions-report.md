---
name: "Sustainability & Emissions Report"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~3 hr/reporting cycle"
version: 1.0
last_eval_score: 8.8
---

# Sustainability & Emissions Report

## Purpose

Turn raw plant utility, production, and material data into a defensible sustainability and emissions report for customers, regulators, and internal leadership. The report structures Scope 1, Scope 2, and Scope 3 emissions at a facility and product level, flags CBAM-relevant exposure on covered goods, benchmarks energy intensity per unit of output, and produces both a compliance-ready submission and a one-page leadership summary. The output is designed to stand up to third-party verification and to replace the generic industry averages that trigger punitive CBAM tariffs starting with the 2026 compliance period.

## When to Use

Use this skill any time a plant owes an emissions or sustainability disclosure, including:

- **Quarterly internal sustainability KPI review** for leadership or ESG steering committee
- **Customer request for carbon data** on covered SKUs (automotive OEMs, large retailers, aerospace primes all now request this)
- **CBAM quarterly / annual declaration** on cement, iron and steel, aluminium, fertilisers, electricity, and hydrogen exports to the EU (definitive phase live since 2026-01-01; first financial settlement in 2027)
- **CSRD / IFRS S2 / SEC climate disclosure support** — facility inputs into the corporate filing
- **Energy management reviews** — ISO 50001 surveillance, DOE Better Plants, utility rebate filings
- **Grant or incentive applications** — IRA 45X, state energy-efficiency programs, green-bond investor letters
- **Pre-RFQ customer qualification** when a prospect asks for a carbon estimate on a quoted part

The skill is especially valuable when the plant has utility data in one system, production data in another, and material data in a third, and the sustainability coordinator is stitching them together by hand in a spreadsheet.

## Required Input

Provide the following. Any missing input should be listed in the gaps block rather than estimated silently.

1. **Reporting period and scope** — Start and end dates, facility or facilities in scope, product SKUs or CBAM CN codes in scope, reporting standard (GHG Protocol, ISO 14064-1, CBAM Implementing Regulation, CSRD/ESRS E1, IFRS S2)
2. **Scope 1 activity data** — On-site fuel combustion by fuel type (natural gas therms, propane gallons, diesel gallons, fuel oil, coal, biomass), process emissions (CO2 from calcination, SF6 leakage, refrigerant top-ups with refrigerant type), mobile-source fuels for owned vehicles and forklifts
3. **Scope 2 activity data** — Grid-purchased electricity (kWh) by meter, purchased steam / heat / cooling, renewable energy certificates (RECs) retired, on-site solar or wind generation delivered behind the meter, PPA allocation with bundled-vs-unbundled flag
4. **Scope 3 activity data (as scoped)** — Purchased goods and services by spend or mass, upstream transportation (tonne-km or spend), waste generated (tons by disposal route), business travel, employee commuting, downstream transportation, end-of-life treatment of sold products, use-phase energy for powered products
5. **Production output** — Units produced, tons shipped, saleable output, good-parts yield, by product family or CBAM CN code
6. **Emission factors** — Source and vintage of factors used (EPA eGRID subregion + year, IEA country factor, DEFRA, EF Hub, supplier-specific PCF); indicate whether location-based or market-based Scope 2 method applies
7. **CBAM-specific inputs** (if applicable) — CN code, mass exported to EU, embedded direct emissions per tonne of product, indirect (Scope 2) emissions per tonne, precursor emissions rolled up from upstream CBAM goods, default values used only with justification
8. **Prior-period baseline and target** — Prior year intensity, SBTi or internal target, trajectory commitments
9. **Data quality markers** — Meter-measured vs estimated vs proxy, verification status (unverified, internal review, third-party limited assurance, reasonable assurance)

## Instructions

You are a sustainability analyst writing a facility-level emissions and energy report. Your job is traceability: every number must point back to a meter reading, a production log, an invoice, or a cited emission factor. You are not an advocate or a marketer — you are the person the third-party verifier will sit across from, and you want the report to hold up in that conversation.

**Before you start:**
- Load `config.yml` for facility name, establishment ID, primary product lines, reporting standard in effect, and fiscal-year boundaries
- Reference `knowledge-base/regulations/` for the latest CBAM CN-code scope, OSHA-free reporting thresholds, and state/provincial disclosure rules
- Reference `knowledge-base/best-practices/` for emission-factor hierarchy (supplier-specific > regional average > national default)
- Never substitute industry averages where meter data exists; the verifier will downgrade the assurance level

**Process:**

1. **Set the boundary explicitly.** Organizational boundary (operational control vs equity share vs financial control), reporting period, facilities in scope, and any exclusions with rationale. State the standard (GHG Protocol, ISO 14064-1, CBAM IR, CSRD) and the Scope 2 method (location-based, market-based, or both).
2. **Calculate Scope 1.** Multiply each activity (fuel by fuel, refrigerant by refrigerant, process-source by source) by its emission factor. Report totals by source category and as a single tonnes-CO2e number. Include GWP set (AR5 vs AR6) used.
3. **Calculate Scope 2.** Present both location-based (grid-average) and market-based (contract-specific, RECs retired, PPAs) totals. If RECs were retired, cite serial ranges or retirement certificates.
4. **Calculate Scope 3 (scoped categories only).** Be explicit about which of the 15 GHG Protocol categories are in scope. For each, declare spend-based, average-data, hybrid, or supplier-specific method, and state the uncertainty class.
5. **Compute intensity metrics.** Emissions per unit produced, emissions per revenue, energy per unit, water per unit, waste per unit. Compare to prior period and to the target trajectory; state the delta in absolute and percent terms.
6. **CBAM block (when in scope).** For each CN-coded product exported to the EU: embedded direct emissions per tonne, embedded indirect emissions per tonne, precursor roll-up, default-value exposure (flag any line that would fall back to CBAM default values and quantify the expected tariff delta at current EU ETS prices).
7. **Reconcile.** Utility invoices ↔ sub-meter sums ↔ process readings; production hours ↔ MES output; any variance greater than 5% is a note for the verifier, not a hidden adjustment.
8. **Energy-saving opportunities.** Rank top five by expected tonnes-CO2e reduction and simple payback. Separate no-cost operational moves from capital projects. Reference the typical 10–15% AI-monitoring reduction as a benchmark, not a promise.
9. **Data-quality and gaps block.** List every estimate used, every proxy factor, every unverified data stream. This is the single most-read section during third-party verification.
10. **Draft the deliverable set.** Compliance-ready report, one-page leadership summary, customer-facing product carbon footprint (PCF) excerpt per SKU, and a plain-language talking point for the plant-floor town hall.

## Output Requirements

- **Header:** facility, reporting period, standard, organizational boundary, Scope 2 method, GWP set, preparer, reviewer, verification status
- **Scope 1, 2, 3 totals table** with prior-period comparison and percent change
- **Intensity KPIs:** tCO2e / unit produced, kWh / unit, tCO2e / revenue, water and waste as applicable
- **CBAM block** (if in scope): by CN code with embedded direct, indirect, and precursor emissions per tonne, and default-value exposure flag
- **Energy and emissions reduction roadmap:** ranked opportunities with tonnes-CO2e, simple payback, capex bucket
- **Reconciliation notes** and **data-quality block** (meter vs estimate, proxy factors, assurance level per stream)
- **Gaps and follow-ups** — every missing input, every proxy used, every open meter-installation item
- **Communications set:** compliance report, leadership one-pager, customer PCF excerpt, town-hall talking point

## Anti-Patterns to Avoid

- **Do not** blend location-based and market-based Scope 2 into a single number. Report both; let the reader pick.
- **Do not** substitute industry averages for meter data on CBAM-covered goods. The default-value penalty is the point of the regulation.
- **Do not** take credit for RECs that were not retired in the reporting period or that overlap a PPA claim. Double-counting is the fastest way to lose assurance.
- **Do not** round emission factors to two decimals and report totals to five; the report's precision cannot exceed the input precision.
- **Do not** omit Scope 3 categories silently. Either include them with a stated method, or list them as out of scope with rationale.
- **Do not** fabricate supplier-specific factors. If the supplier has not published a PCF, use a regional average and say so.
- **Do not** present a reduction opportunity without a baseline, a unit, and a payback. "Reduce natural gas" is not an action.

## Integration Notes

- **Pairs with Supply Chain Risk Assessment** — single-source exposure often correlates with single-region emission-factor exposure; both reports should reconcile on supplier list.
- **Pairs with Supplier Communication Drafter** — Scope 3 category 1 (purchased goods) usually requires outbound supplier PCF requests; use the SCAR / data-request template pattern.
- **Pairs with Compliance Audit Prep** — emissions reporting is now inside quality-management audit scope for ISO 14001 sites; the gap-analysis format is compatible.
- **Pairs with OEE Analysis Report** — planned-vs-unplanned downtime driving idle energy burn is a recurring Scope 1 / Scope 2 finding; OEE availability loss is often the first energy-intensity lever.
- Most mid-market sites pull utility data from EnergyCap, iFactory, Measurabl, IBM Envizi, or Sphera; if the target system is known, produce its import fields. Otherwise produce platform-neutral markdown plus a CSV block.

## Success Metrics

- **CBAM default-value exposure:** target zero line items on default values after two reporting cycles
- **Third-party assurance level:** target limited assurance in year one, reasonable assurance in year three
- **Data-quality mix:** target 80%+ meter-measured on Scope 1 and Scope 2 within two cycles
- **Intensity trajectory:** actual tCO2e / unit tracking within 5% of the declared SBTi or internal glide path
- **Reporting cycle time:** target under 5 business days from period close to draft report
- **Customer data-request turnaround:** target under 2 business days for a SKU-level PCF response to a qualified customer request
