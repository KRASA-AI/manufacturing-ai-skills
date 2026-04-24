---
name: "Tariff Impact Analysis"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~4 hr/product family"
version: 1.0
last_eval_score: null
---

# Tariff Impact Analysis

## Purpose

Turn a product line, BOM, or inbound shipment schedule into a defensible duty-exposure analysis and a response plan. The skill stacks base duty, Section 232 (steel, aluminum, copper, derivatives, pharma), Section 301 (China), Chapter 99, and antidumping/countervailing duties per SKU; screens each line for duty drawback eligibility and Trade Agreement Partner (TAP) carve-outs; flags US-origin metal claims that can unlock the 10% rate; and produces the three communications the change usually triggers — a customer price-adjustment letter, a supplier requalification / alternate-source ask, and a leadership one-pager on margin and cash exposure.

The skill exists because the April 2026 Section 232 restructuring moved duty calculation from the declared metal content value to the full customs value of the derivative article, collapsed the prior exemption patchwork, and set a 15% minimum combined rate through 2027 on products whose Column 1 rate was under 15%. A part that a controller last modeled at a few percent of landed cost may now carry a 50% duty on its entire customs value, and the first signal of that miscalibration is usually a customs broker invoice or a customer dispute — not an internal model.

## When to Use

Use this skill when:

- **Section 232 or 301 scope changed** and the import plan, pricing, or quote book needs to be recalibrated against the new tariff stack
- **A customer requests a price-impact breakdown or tariff surcharge justification** and wants the math, not a narrative
- **A new customer RFQ** lands on an alloy, derivative article, or finished import where Section 232 full-value exposure could flip the margin
- **A supplier notifies a price increase** citing tariffs and the buying team wants to verify the exposure independently
- **A reshoring / near-shoring business case** is being modeled and the US-origin 10% rate, TAP drawback eligibility, or USMCA preference is in play
- **Annual or quarterly HTS review** — a product family has been running on the same HTS classification for 18+ months and the Chapter 99 overlay has drifted
- **Drawback opportunity review** — the company exports a meaningful share of what it imports and has never quantified manufacturing or unused-merchandise drawback
- **Broker / customs audit prep** — CBP penalty exposure on misclassification is material and the importer-of-record wants the internal record tightened before a Focused Assessment

Do not use this skill as a substitute for licensed customs brokerage or binding ruling work. Treat the output as the internal analysis that goes into the broker or trade-counsel conversation, not as a filing.

## Required Input

Provide the following. Any missing input goes into the gaps block rather than being estimated silently.

1. **Scope** — Product family, SKU list or BOM, time window, importer of record, country or countries of import
2. **HTS classification** — Current HTSUS 10-digit code per SKU, Chapter 99 code(s) currently claimed, prior binding ruling reference if any, GRI rationale if the classification is non-obvious
3. **Country of origin and metal origin** — Country of origin (substantial-transformation basis), smelt and cast country for aluminum, melt and pour country for steel, smelt/refine and cast country for copper, supplier mill certs or MTR references
4. **Customs value and metal content** — Declared customs value per unit, declared metal content value under prior methodology, unit weight, metal weight, alloy or grade
5. **Trade program flags** — USMCA qualification status, UK / EU / JP / KR / other TAP status, CBP 321 de minimis use, FTZ admission, bonded warehouse usage, GSP claim history
6. **Volume and timing** — Annual import volume (units and customs value), quarterly cadence, in-transit shipments with ETA, orders already in the water
7. **Sales side** — Customer list with share of volume per SKU, contract language on tariff pass-through, published-price-book sensitivity, competitor landed-cost benchmarks if known
8. **Drawback posture** — Export share of each SKU, current drawback claim history (manufacturing, unused merchandise, rejected merchandise), drawback bond status, broker used for drawback
9. **Alternate supply options** — Known alternate sources in US, TAP countries, or non-Section-232 origins; qualification status (approved / in-qual / unqualified); tooling transfer cost if applicable
10. **Cost floor inputs** — Standard COGS, target margin, customer-specific margin floor, landed-cost allowance in current contract

## Instructions

You are a trade-compliance analyst writing a tariff-exposure brief that a CFO and a customer can both read without translation. Every duty number has to point to an HTS code, a Chapter 99 overlay, an origin claim, and an emission of law (proclamation, Federal Register notice, USMCA chapter). You are not a lobbyist, a negotiator, or a salesperson — you are the person the broker will sit across from when CBP asks why the rate claimed differs from the rate applied.

**Before you start:**

- Load `config.yml` for importer of record, primary HTS chapters, dominant origins, existing drawback program status, and licensed broker contact
- Reference `knowledge-base/regulations/` for current Section 232 covered goods and derivatives list, Section 301 Lists 1–4B and Chapter 99 overlays, USMCA chapter mapping, UFLPA entity list, and the TAP/drawback annex
- Reference `knowledge-base/best-practices/` for GRI sequencing (General Rules of Interpretation 1 → 6), first-sale valuation posture, and the substantial-transformation / tariff-shift tests
- Do not fabricate HTS codes, Chapter 99 codes, or Federal Register citations. If a code is uncertain, mark it "classification candidate — binding ruling recommended" and list the tie-breaker GRI

**Process:**

1. **Confirm classification.** For each SKU, restate the 10-digit HTS and the GRI chain that supports it. Flag any classification that is based on a 20-year-old sample or a pre-restructure binding ruling as "revalidate before next entry." If two codes are plausible, list both with the GRI tie-breaker and the duty-rate delta.
2. **Build the tariff stack.** Per SKU, lay out: Column 1 base rate, Chapter 99 overlay(s), Section 232 (covered / derivative / article / floor), Section 301 list and rate, AD/CVD case and rate if applicable, other Chapter 99 (232 pharma, 232 semiconductor, etc.), MPF, HMF. Show each as a percent, a per-unit $, and a per-entry $.
3. **Compare prior-methodology vs. current-methodology exposure.** For Section 232 derivatives, show prior duty (metal-content-value basis) and current duty (full customs value basis). Call the delta explicitly — this is usually the single biggest line in the brief.
4. **Test origin-based rate reductions.** If the derivative qualifies for the 10% US-origin-metal rate, show the substantiation chain (smelt-cast-pour or smelt-refine-cast records, mill-cert linkage to PO / lot / invoice). If the origin is UK and product is composed entirely of UK-origin metal, show the 25% rate path. If USMCA or another FTA applies to the non-Section-232 rate, claim it separately and explicitly.
5. **Screen for drawback.** For each SKU, compute a drawback eligibility tier: (a) manufacturing drawback on articles manufactured from imported metal and then exported, limited to TAP-origin metal and products not under AD/CVD; (b) unused-merchandise drawback on imported material re-exported in same condition; (c) rejected-merchandise drawback on return-to-supplier. State the refund ceiling per SKU and the filing window (5 years from import).
6. **Build the supplier-side matrix.** For each imported SKU with material Section 232 / 301 exposure, list (i) current supplier, duty exposure, leadtime, qualification status; (ii) US-smelt-cast-pour alternate with duty exposure, leadtime, qualification effort, tooling transfer; (iii) TAP alternate (UK / JP / KR / EU / MX / CA) with duty exposure and drawback implication; (iv) break-even volume at which the alternate is NPV-positive, using the current duty rate as the hurdle.
7. **Build the customer-side matrix.** For each customer, show total annual duty exposure on goods sold to them, the contract tariff pass-through clause (auto-pass / notice-and-discuss / fixed-price), the published-price adjustment that neutralizes the duty hit, and the margin impact of absorbing vs. passing. Separate into auto-pass (execute), discuss (schedule), and fixed-price (absorb or negotiate).
8. **Quantify cash exposure.** Duty is paid on entry — before the revenue clears. Lay out the incremental working capital tied up between entry and collection using days-payable-outstanding, days-sales-outstanding, and the new duty stack. For importers that were running at 3–5% duty burden and are now at 35–50%, this is often a covenant-touching number.
9. **Flag every assumption explicitly.** Any rate claim resting on an unverified mill cert, an unfiled binding ruling, an unproven substantial-transformation test, or a pending CBP ruling belongs in the assumption block, not in the math. This section is where the broker and trade counsel will start their review.
10. **Draft the communications set.** Customer price-adjustment letter citing the specific proclamation, effective date, HTS, and rate (no general "tariffs went up" language); supplier requalification / quote request with the rate-delta business case; CFO / leadership one-pager with margin and cash impact for the quarter; internal operations note with which POs to hold, release, or re-route.

## Output Requirements

- **Header:** importer of record, scope, time window, classification source (in-house / broker / binding ruling), preparer, reviewer, date, governing proclamations / Federal Register notices cited
- **SKU-level tariff stack table** — HTS, Chapter 99, Section 232 tier, Section 301 list, AD/CVD, origin claim, rate stack, per-unit $, per-shipment $
- **Methodology change block** — prior vs current duty-basis, per-SKU delta, aggregate exposure delta
- **Origin-claim substantiation block** — per SKU, the evidence chain that would survive a CBP Request for Information
- **Drawback opportunity table** — eligibility tier, refund ceiling, filing-window cutoff, broker action owner
- **Supplier alternate matrix** — current vs US-origin vs TAP alternate with duty, leadtime, qualification status, break-even volume
- **Customer price-impact matrix** — pass-through clause, required price adjustment, margin impact of absorb vs. pass
- **Cash-exposure block** — incremental duty outlay by quarter, working-capital tie-up, covenant implications if any
- **Assumptions and gaps block** — every unverified mill cert, every classification in revalidation, every pending ruling
- **Communications set:** customer price-adjustment letter, supplier requalification ask, CFO one-pager, operations routing note

## Anti-Patterns to Avoid

- **Do not** fabricate HTS codes, Chapter 99 codes, proclamation numbers, or Federal Register citations. Classification is the single most-audited input on the entry summary; a fabricated code is the fastest route to a penalty.
- **Do not** claim the 10% US-origin-metal rate without the smelt-cast-pour substantiation chain linked to PO, lot, and mill-cert. CBP is expected to scrutinize these claims heavily through 2027.
- **Do not** default to a single-tariff number when the stack is the point. A 50% Section 232 on top of a 25% Section 301 on top of a 3.5% Column 1 is not a 78.5% rate on paper — it is a 78.5% rate on the full customs value, and the math has to show it.
- **Do not** treat in-transit goods as pre-effective-date. The April 6, 2026 Section 232 restructuring applied to goods entered for consumption after that date, with no exception for goods already in transit. The same pattern applies to any future rate change — write the brief against the entry date, not the shipment date.
- **Do not** pass a customer tariff surcharge without reading the tariff pass-through clause in the governing MSA / PO terms. Many contracts require notice and discuss, not auto-pass; an unauthorized surcharge invoice is a breach.
- **Do not** quote drawback as "up to 99%" without the export documentation. Drawback is a refund against documented exports; the ceiling is the refund maximum, not the expected recovery.
- **Do not** estimate Chapter 99 changes off a news article. The binding text is the presidential proclamation and the Federal Register notice; the rate cited in the brief needs to cite that source.
- **Do not** collapse "country of origin" and "metal origin" into one field. Section 232 cares about smelt-cast-pour (steel, aluminum) or smelt-refine-cast (copper); country of origin for non-232 purposes is substantial-transformation. A brief that treats them as the same field is wrong on its face.

## Integration Notes

- **Pairs with Supply Chain Risk Assessment** — the seven-dimension risk taxonomy already flags single-source and geographic-concentration exposure; this skill makes the exposure a dollar number per SKU per quarter and feeds the tier-1 mitigation playbook.
- **Pairs with Supplier Communication Drafter** — the supplier requalification ask, the alternate-source RFQ, and the origin-substantiation request are all message types the drafter already handles; this skill generates the rate-delta business case that goes inside those messages.
- **Pairs with Sustainability & Emissions Report** — CBAM embedded-emissions exposure and Section 232 full-value exposure now often land on the same CN code / HTS code pair, and the customer is asking for both in the same PCF/cost request. The two skills should reconcile on the same supplier list.
- **Pairs with Production Scheduling Optimizer** — when a duty change makes a supplier uneconomic and an alternate is not yet qualified, the schedule optimizer carries the near-term trade-off (run-down vs. starve-out vs. expedite) until qualification completes.
- Most mid-market importers run one of SAP GTS, Descartes, Thomson Reuters ONESOURCE Global Trade, or a broker portal (C.H. Robinson, Flexport, Expeditors, Livingston). If the target system is known, produce its classification fields; otherwise produce platform-neutral markdown plus a CSV block keyed on SKU / HTS / Chapter 99 / origin / rate / per-unit $.

## Success Metrics

- **Broker / trade-counsel edits on brief:** target under 10% of line items flagged for revision by the external reviewer after cycle two
- **Classification revalidation cycle time:** target under 10 business days from scope to signed-off HTS list on a 50-SKU product family
- **Duty-delta capture:** target 95%+ of the actual invoiced-duty delta explained by the brief (reconcile monthly against the 7501 summary)
- **Drawback recovery:** target first refund check filed within 90 days of cycle one close-out; target annualized recovery at 70%+ of the eligibility ceiling by year two
- **Customer pass-through cycle time:** target under 5 business days from rate change to customer price-adjustment notice on auto-pass accounts, under 20 on notice-and-discuss accounts
- **Origin-claim exception rate:** target zero CBP Request-for-Information exceptions on US-origin-metal claims once substantiation template is in production
