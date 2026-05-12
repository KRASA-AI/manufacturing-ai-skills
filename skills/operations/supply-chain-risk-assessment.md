---
name: "Supply Chain Risk Assessment"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/assessment + avoided line-down hours"
version: 2.0
last_eval_score: 8.7
---

# Supply Chain Risk Assessment

## Purpose

Turn a tiered supplier list, material criticality map, and current disruption inputs into a defensible supply-chain risk assessment that (a) scores every critical supplier and material on a seven-dimension risk taxonomy, (b) assigns a risk tier using an explicit Likelihood × Severity matrix, (c) flags CBAM-covered goods and forced-labour jurisdiction exposure before customers or regulators find them, (d) produces mitigation playbook recommendations tiered from dual-sourcing to contract clauses, and (e) defines leading-indicator contingency triggers so disruption response is pre-decided rather than improvised. The output is the artifact that sits in the quarterly risk review, feeds the supplier-development roadmap, and gets attached to customer RFQ responses that require a supply-chain-continuity story.

## When to Use

Use this skill when:

- **Quarterly supply-chain risk review** with operations, procurement, and quality leadership
- **New supplier onboarding** — before PPAP approval, run the full seven-dimension assessment on the candidate
- **Customer audit or CSR** — OEM customer-specific requirements (GM / Ford / Stellantis / Boeing / Airbus / medical primes) increasingly demand a documented risk assessment on tier-1 and tier-2 exposure
- **Post-disruption review** — after a specific event (port closure, supplier fire, semiconductor allocation, currency swing) update the assessment and reset triggers
- **Annual strategic-sourcing plan** — roll up the critical-material list with dual-source and long-lead positioning
- **ISO 9001 clause 8.4 / IATF 16949 clause 8.4.2.1 audit prep** — external providers risk assessment is an explicit clause requirement
- **CBAM 2026 definitive-phase exposure** — iron and steel, aluminium, cement, fertilisers, hydrogen, and electricity sourcing needs an embedded-emissions and default-value exposure read, not just a logistics read
- **Forced-labour compliance scan** — UFLPA Entity List, Xinjiang exposure, EU Forced Labour Regulation (in force 2027) require tier-2 visibility
- **Pre-RFQ customer qualification** — when a prospect asks for a continuity-of-supply statement on a quoted part family

This skill is an **advisory** artifact. It does not replace supplier-quality audits or financial-health subscriptions — it is the synthesis layer that combines those inputs with the plant's own bill-of-materials and production plan.

## Required Input

Provide the following. Anything missing goes into the gaps block, not a guess.

1. **Supplier roster** — Supplier name, DUNS / registration ID, tier (1 / 2 / 3 as known), country of origin, country of manufacture (can differ from country of origin), plant address when known, parent-company ownership, ISO / IATF / AS9100 / ISO 13485 certification status and expiry, OEM / industry approval status
2. **Material / part criticality** — For each supplied item: part number, material family, spend per year, annualised volume, current on-hand days-of-supply, single-sourced vs dual / multi-sourced, time-to-qualify an alternate (weeks), customer-specified supplier (Yes / No + which customer), tooling ownership
3. **Performance history** — On-time-in-full (OTIF) % by supplier for the trailing 12 months, PPM defect rate, accepted-at-IQC %, SCAR count and response time, late-delivery count, premium-freight incidence
4. **Financial signals** (from D&B, Rapid Ratings, CreditSafe, or equivalent, where available) — financial-stability tier, trend, recent watch-list events, bankruptcy filings, restructuring indicators, DSO drift; note subscription source and vintage
5. **Geographic exposure** — Country risk rating, natural-disaster exposure (seismic zone, flood zone, cyclone / typhoon window, wildfire zone), labour-relations cadence (contract expiry, strike history), port / canal / chokepoint dependence (Suez, Panama, Red Sea, Taiwan Strait, Bosphorus, Mississippi lock system), cross-border crossings in the routing
6. **Regulatory / ESG flags** — CBAM CN-code exposure for EU-bound iron / steel / aluminium / cement / fertilisers / hydrogen, UFLPA Entity List hits, Xinjiang Uyghur Autonomous Region (XUAR) sourcing flag, EU Forced Labour Regulation exposure, conflict-minerals (3TG) smelter status, REACH / RoHS status, sanctions (OFAC SDN, UK HMT, EU consolidated list, Entity List)
7. **Current disruption context** — Named events in motion (strike, fire, flood, sanction, allocation, currency move), customer campaign commitments at risk, any open escalation with a supplier
8. **Customer CSR requirements** — Which customers have imposed their own risk framework (GM SPQR, Ford Q1, FCA / Stellantis Basel-IV adjacent requirements, Boeing D1-9000, Airbus SQIP, medical-device supplier risk)

## Instructions

You are a supply-chain risk analyst writing an assessment that the CPO will defend in the quarterly operating review, that the plant manager will use to sequence dual-source projects, and that a customer auditor may request verbatim. Your job is to turn a supplier list and a BOM into ranked, tiered, defensible risk, with explicit triggers and a mitigation playbook — not a generic SWOT.

**Before you start:**
- Load `config.yml` for plant name, critical-material list, tiered supplier list, CBAM exposure flag, customer CSR list, currency of record, voice preferences, and financial-data subscription sources
- Reference `knowledge-base/regulations/` for the current CBAM CN-code scope, UFLPA Entity List, EU Forced Labour Regulation scope, conflict-minerals smelter list, and sanctions consolidated lists
- Reference `knowledge-base/terminology/` for correct tier / OEM / automotive / aerospace / medical terminology
- Use voice from `config.yml` → `voice`

**Process:**

1. **Scope the assessment.** State which supplier population and material population are in scope. Separate critical / strategic / leverage / routine (Kraljic quadrants are useful even if not named). Be explicit about tier-1-only vs multi-tier — if the assessment is tier-1-only, say so, because the biggest risks (semiconductor substrates, rare-earth magnets, polysilicon) almost always live two or three tiers deep.

2. **Score each supplier / material on the seven-dimension risk taxonomy.** Each dimension gets a 1–5 score with a one-sentence justification:
   - **Single-source exposure** — number of approved sources for this part / material, alternate qualification time (weeks), tooling ownership
   - **Geographic concentration** — country + plant + logistics-chokepoint concentration
   - **Lead-time volatility** — trailing-12-month lead-time range vs commitment; every premium-freight incident counts
   - **Quality history** — PPM, SCAR count, escape rate, customer-complaint traceability to this supplier
   - **Financial stability** — subscription-sourced rating + trend; DSO drift; recent events
   - **Geopolitical exposure** — sanctions / tariffs / export-control listing, country risk rating, sector-specific allocation exposure
   - **Regulatory / ESG** — CBAM embedded-emissions exposure (iron, steel, aluminium, cement, fertilisers, hydrogen, electricity), UFLPA / forced-labour exposure, REACH / RoHS, conflict-minerals smelter status

3. **Apply the Likelihood × Severity matrix.** Likelihood (1–5) is the aggregate of the seven-dimension taxonomy scores, normalised. Severity (1–5) is the production-impact consequence: line-down hours, customer-penalty exposure, safety / regulatory consequence, brand exposure. The cell determines risk tier:
   - **Critical (red)** — L ≥ 4 AND S ≥ 4 → executive review, immediate mitigation project
   - **High (orange)** — L × S in 12–16 cells excluding critical → named owner, mitigation in < 90 days
   - **Moderate (yellow)** — L × S in 6–10 cells → watch-list with quarterly review
   - **Low (green)** — L × S ≤ 5 → routine management

4. **Map tier-2 exposure on every Critical and High item.** Name the sub-tier (raw material, key component, sub-assembly) and the country of manufacture. If tier-2 is not visible, say "tier-2 not visible — supplier visibility project required" as a gap, do not paper over it. Tier-2 visibility is the biggest single determinant of whether the assessment is worth the paper it is printed on.

5. **Flag CBAM exposure explicitly.** For every in-scope item on an EU-bound product that is CN-code covered, estimate embedded-emissions exposure, flag default-value exposure at current EU ETS price, and pair with Sustainability & Emissions Report to close the data loop. Default-value exposure is a cash-flow risk, not just a compliance line.

6. **Flag forced-labour exposure explicitly.** Any supplier with XUAR sourcing, any hit on the UFLPA Entity List, any component known to source polysilicon / cotton / tomato / silica / aluminium from XUAR gets a named risk line with action required. EU Forced Labour Regulation enforcement begins 2027 and applies retroactively to products placed on the market — the time to fix is now, not when the first detention order lands.

7. **Define contingency triggers as leading indicators, not lagging ones.** A trigger is an observable signal that activates a pre-approved response:
   - Leading: *OTIF drops below 85% for two consecutive weeks* → activate safety-stock draw + supplier escalation
   - Leading: *DSO on supplier invoices extends > 15 days beyond contract terms* → activate financial-watch flag, reduce forward commitments
   - Leading: *Lead time on critical part extends > 2σ above baseline* → activate second-source qualification acceleration
   - Lagging (still useful): *Supplier files for administration / Chapter 11* → activate crisis response playbook, notify customer per contract
   Every trigger has an owner and a pre-defined response — the assessment is worthless if the response starts with "then we'll talk about it."

8. **Build the mitigation playbook.** For each Critical and High item, select from a tiered playbook — do not propose full dual-sourcing on every item:
   - **Dual / multi-source qualification** (biggest lever; longest lead time — typically 12–24 weeks for PPAP-grade)
   - **Safety stock adjustment** (fastest lever; working-capital cost)
   - **Inventory hedging / forward-position buying** on commodity-priced inputs (steel, copper, aluminium, resin, rare earths)
   - **Contract clauses** — force majeure, allocation rights of first refusal, price adjustment, min-commit / max-order, capacity reservation
   - **Supplier development project** — co-invest in capacity, quality, tooling duplication, or dual-plant qualification
   - **Vertical integration** or in-sourcing (last-resort lever on strategic items)
   - **Customer-approved alternate** — for customer-specified suppliers, the mitigation has to be customer-driven
   State the lever, the owner, the expected time-to-benefit, and the cost / working-capital impact.

9. **Quantify the top-5 risks in production-impact terms.** For each: days-of-supply at current burn rate, line-down scenario (hours × gross margin per hour = exposure), customer-penalty / PPAP-resubmission exposure, customer-specific-programme exposure (name the programme). Avoid hand-waving.

10. **Gaps, assumptions, and data-quality block.** List every supplier without financial-stability data, every tier-2 not visible, every CBAM line without supplier-specific emissions data, every forced-labour exposure without tier-2 confirmation, every financial subscription that is more than 6 months old.

## Output Requirements

- **Header:** plant, assessment period, author, scope (supplier / material population), customer CSR frameworks invoked, reference standards (ISO 9001 8.4, IATF 16949 8.4.2.1, AS9100 8.4, ISO 13485 7.4)
- **Executive summary** — count of Critical / High / Moderate / Low, top-5 risks in one line each, quarter-over-quarter direction, CBAM exposure line, forced-labour exposure line
- **Seven-dimension score table** per critical / high supplier with 1–5 per dimension and one-sentence justifications
- **Likelihood × Severity matrix** visualisation (markdown table or ASCII 5×5) with each item placed in its cell
- **Tier-2 visibility map** for Critical / High items
- **CBAM exposure block** with CN codes, supplier, embedded-emissions estimate, default-value cash exposure at current EU ETS price, proposed mitigation
- **Forced-labour exposure block** with supplier, country, XUAR-exposure rationale, UFLPA / EU-FLR status, remediation path or dual-source plan
- **Contingency trigger table** — trigger (leading / lagging), owner, pre-defined response, last-tested date
- **Mitigation playbook** — per Critical / High item, lever, owner, time-to-benefit, cost / working-capital impact
- **Top-5 quantified production-impact table** — days-of-supply, line-down scenario, penalty exposure, customer programme exposure
- **Gaps and assumptions** — every missing dimension, every unverified tier-2, every subscription vintage > 6 months

## Anti-Patterns to Avoid

- **Do not** report a tier-1-only assessment as comprehensive. Name the tier-2 gap explicitly. The biggest supply disruptions of the last five years — semiconductor substrates, neon for lithography, rare-earth magnets, polysilicon for solar / power electronics — were tier-2 or deeper.
- **Do not** equate supplier diversity with supply-chain resilience. Two suppliers in the same industrial cluster, same chokepoint, same sub-tier is one risk, not two.
- **Do not** list "geopolitical risk" as a generic dimension without naming the specific sanction list, export-control regime, or tariff schedule that applies.
- **Do not** assume default CBAM values are the endpoint. Default values are punitive and designed to move importers to supplier-specific data; frame them as a lever, not a cost centre.
- **Do not** bury forced-labour exposure in a "regulatory" bullet. UFLPA detentions and EU Forced Labour Regulation have specific, named legal consequences; they get their own block.
- **Do not** recommend dual-sourcing every critical item. Dual-sourcing has working-capital, tooling, and quality-variation costs; use the tiered playbook.
- **Do not** define triggers as "when it becomes a problem." A trigger is an observable leading indicator with a pre-approved response.
- **Do not** fabricate financial ratings or certification status. Cite the subscription source and vintage, or mark as "unverified."
- **Do not** present the output as a final answer on a customer-specified supplier. Customer-specified suppliers (CSS in automotive) have a constrained mitigation set that requires customer approval.

## Integration Notes

- **Pairs with Sustainability & Emissions Report** — the CBAM block is shared: single-source exposure often correlates with single-region emissions-factor exposure; the two reports should reconcile on supplier list and CN-code scope.
- **Pairs with Supplier Communication Drafter** — any Critical / High supplier gets a parallel communication track (SCAR, scorecard feedback, delivery escalation, or capacity-reservation letter) drafted from the relevant template.
- **Pairs with Compliance Audit Prep** — ISO 9001 8.4 and IATF 16949 8.4.2.1 external-providers risk is a direct input to the audit-readiness file.
- **Pairs with Production Scheduling Optimizer** — a material flagged Critical on days-of-supply should not be scheduled into a campaign past its days-of-supply horizon without a named mitigation.
- **Pairs with CAPA Document Builder** — a repeated supplier-caused escape opens a CAPA on the supplier-approval process, not just on the supplier.
- **Pairs with Warranty Claim Analyzer** — supplier-caused warranty clusters feed back into the quality-history dimension.
- Most mid-market sites source inputs from Coupa Risk Assess, Riskmethods (now Sphera), Interos, Resilinc, Everstream Analytics, D&B Supplier Risk, Rapid Ratings, or an internal combination of Excel + D&B + customer scorecards. If the target system is known, produce its field layout. Otherwise produce platform-neutral markdown with a CSV-compatible supplier-risk table.

## Success Metrics

- **Tier-2 visibility:** target > 70% of Critical items with named tier-2 supplier, country, and capacity within one year
- **Dual-source coverage:** target 100% of Critical items with either dual-source or a qualified-alternate-ready-to-activate state within 18 months
- **Trigger-to-response time:** target < 5 business days from trigger fire to response action (not just notification)
- **CBAM default-value exposure:** target zero covered lines on default values after two reporting cycles (shared with Sustainability & Emissions Report)
- **Forced-labour exposure:** target zero UFLPA / EU-FLR exposure on any direct or known-tier-2 supplier by the 2027 EU-FLR enforcement date
- **Line-down hours attributable to supply-chain risk:** quarterly trend downward, correlated with the mitigation playbook execution
- **Assessment cycle time:** target < 3 business days from input ready to draft published for quarterly review
