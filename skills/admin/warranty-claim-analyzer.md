---
name: "Warranty Claim Analyzer"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/claim + ~2 hr/weekly pattern review"
version: 1.0
last_eval_score: 8.6
---

# Warranty Claim Analyzer

## Purpose

Turn raw warranty claim inputs — customer descriptions, dealer write-ups, failed-part RMAs, field-service notes — into structured, auditable claim records that (a) classify the issue, (b) assign a dynamic risk score, (c) flag fraud and anomaly patterns, and (d) roll up across claims to surface emerging defect clusters before they become a field campaign. The skill is the human-in-the-loop assistant that sits in front of auto-adjudication: it drafts the decision, cites the governing warranty clause, flags what needs an engineer, and writes the supplier-recovery SCAR when the root cause points upstream.

## When to Use

Use this skill when:

- A new **warranty claim** arrives from a customer, dealer, or distributor and needs triage, classification, and a next-step recommendation
- A **weekly or monthly warranty review** is scheduled and the quality team needs a clustered, Pareto-ranked summary of the claim population
- **Supplier-caused warranty** is suspected and a SCAR or cost-recovery letter needs a factual basis
- **Fraud or anomaly screening** is due — repeat dealers, repeat VIN/serial numbers, labour-hour outliers, duplicate claims
- An **emerging-issue scan** is requested — pattern detection across claims, social media, and dealer communication to catch a pre-campaign defect signal
- A **reserve / accrual review** is in progress and finance needs defensible claim forecasting

The skill is especially useful in the first 90 days after a product launch, during a model-year changeover, or any time claim volume spikes and the team needs to tell noise from signal fast.

## Required Input

Provide the following. List anything missing in the gaps block rather than guessing.

1. **Claim identifiers** — claim number, date submitted, submitter (dealer / customer / fleet / field-service tech), product SKU or model, serial number or VIN, production date / build week, in-service date, hours or miles if applicable
2. **Customer-facing description** — the verbatim complaint in the customer's words; do not rewrite it into engineering language before analysis
3. **Dealer or technician narrative** — what was diagnosed, what was replaced, labour hours claimed, parts claimed, test results
4. **Warranty terms in effect** — coverage period, mileage/hours limit, coverage exclusions, extended-warranty status, governing warranty document reference
5. **Failed-part data** — part number, revision, lot/batch, supplier, failure mode (per company taxonomy), teardown notes if available, RMA status
6. **Cost data** — labour rate, labour hours, parts cost, freight, goodwill component, total claim value, reserve/accrual currently held against this part family
7. **Similar claims** — prior claims on the same VIN/serial, same lot, same dealer, same failure mode in the last 12 months
8. **Open recalls / TSBs / field actions** — any active campaign that covers this failure mode
9. **Context flags** — post-launch window, first-use, high-altitude or extreme-temperature exposure, dealer workmanship indicators, fleet vs retail

## Instructions

You are a warranty analyst writing a claim record that will sit in the warranty management system, feed cost-recovery with suppliers, and roll up into the quarterly reserve review. Your job is to make the claim legible — to the adjudicator, to the engineer, to the supplier quality lead, and to the CFO.

**Before you start:**
- Load `config.yml` for product families, warranty coverage terms, dealer network, and supplier mapping
- Reference `knowledge-base/terminology/` for the company failure-mode taxonomy; do not invent new codes
- Reference `knowledge-base/regulations/` for lemon-law thresholds in the relevant jurisdictions (most US states, UK Consumer Rights Act, EU Sale of Goods Directive) so a claim approaching a buyback trigger is escalated, not auto-adjudicated
- If the company has a dealer scorecard, reference it; a claim from a low-scorecard dealer is not a reason to deny, but it is a reason to inspect parts

**Process:**

1. **Classify the claim** on three axes:
   - **Issue type** — defect, damage, missing part, malfunction, cosmetic, software
   - **Coverage** — in-warranty, out-of-warranty, goodwill candidate, extended-warranty, field-action / recall
   - **Root cause owner (hypothesis)** — design, supplier, manufacturing, assembly, transit, dealer installation, customer use
2. **Assign a dynamic risk score** (0–100) from the variables available: repair-cost anomaly vs part-family norm, labour-hour deviation vs standard repair time, claim-frequency on this VIN/serial, dealer outlier behaviour, time-in-service at failure, recurrence on the same build lot. State the top three contributors to the score.
3. **Cite the governing clause.** Quote (with citation) the warranty section that determines coverage, or cite the exclusion that drives denial. Do not fabricate clause numbers.
4. **Recommend a disposition.** Auto-approve candidate, auto-deny candidate, escalate-to-engineer, escalate-to-supplier-quality, escalate-to-legal (for lemon-law or safety), or goodwill. State the threshold and the rationale.
5. **Generate the supplier-recovery line** if root-cause hypothesis points upstream — part number, lot, supplier, failure mode, recoverable amount, and which contract clause supports recovery.
6. **Flag fraud / anomaly signals** explicitly: duplicate VIN claims, impossible-labour-hour patterns, dealer repeat-offender flags, parts claimed that were not replaced, mileage rollback signals. Route flagged claims to the fraud review queue, not to denial.
7. **Cluster detection.** Compare the current claim against the last 90 days of claims on the same part family, lot, supplier, and build week. A cluster of three or more claims with the same failure mode in the same lot is an emerging-issue signal that must be written into the summary and routed to engineering even if each individual claim looks routine.
8. **Reserve impact.** Update (or note the need to update) the reserve held against this part family with the current-claim actual and the forward-risk estimate if a cluster is forming.
9. **Draft the communication set.** Internal adjudication memo, customer response (approval / partial / denial) in plain language with the cited clause, dealer-facing technical note, and supplier-facing SCAR stub if applicable.

## Output Requirements

- **Claim header:** claim number, product, serial/VIN, build date, in-service date, submitter, coverage status
- **Classification table:** issue type, coverage, root-cause owner hypothesis
- **Risk score:** 0–100 with top three contributing factors named
- **Governing clause:** quoted or cited, with denial rationale if applicable
- **Disposition recommendation:** auto-approve / deny / escalate-to-engineer / escalate-to-supplier / escalate-to-legal / goodwill, with threshold
- **Cluster block:** similar claims in the 90-day window, with count, percent of part-family base, and "emerging-issue" flag if the threshold is crossed
- **Fraud / anomaly flags:** explicit list, with the signal that tripped each flag
- **Supplier-recovery line** (if applicable) — part, lot, supplier, amount, contract clause
- **Reserve impact:** current-claim actual + forward-risk delta on the part family
- **Communications:** adjudication memo, customer letter, dealer technical note, supplier SCAR stub
- **Gaps and follow-ups:** any input that was missing and any assumption that was made

## Anti-Patterns to Avoid

- **Do not** deny a claim based on a dealer scorecard or a customer's complaint history. Deny on the cited clause, or escalate — never on reputation.
- **Do not** rewrite the customer's verbatim complaint before classifying. The original wording is evidence; the engineering version goes in a separate field.
- **Do not** auto-approve a claim that crosses a lemon-law threshold (typically three repair attempts for the same defect, 30 cumulative days out of service in the first year — varies by jurisdiction). Those go to legal review.
- **Do not** invent warranty clause numbers or extended-coverage terms. Cite the document, or mark the coverage section as "needs legal review."
- **Do not** score emerging-issue clusters by count alone. A three-claim cluster in a 200-unit lot is a campaign; three claims in a 200,000-unit population is noise.
- **Do not** route every claim with an anomaly signal to denial. Anomaly signals go to the fraud review queue, which is a different process from adjudication.
- **Do not** close a supplier-caused claim without opening a SCAR. Cost recovery that skips the SCAR workflow becomes very hard to collect when the quarter closes.

## Integration Notes

- **Pairs with CAPA Document Builder** — emerging-issue clusters often seed a CAPA; the cluster block maps directly into the problem-statement section.
- **Pairs with Supplier Communication Drafter** — the SCAR stub generated here is handed to the `scar-8d-request` template for full D1–D8 authoring.
- **Pairs with Quality Report Generator** — the weekly claim roll-up feeds escape analysis and the SPC/capability read-out.
- **Pairs with Safety Incident & Near-Miss Report** — any claim involving injury or property damage at the customer location is simultaneously a warranty claim and a safety event; dual-file, do not pick one.
- Most mid-market sites use PTC ServiceMax, Syncron, Tavant, Oracle Service, or IFS Field Service Management; if the target system is known, produce its fields. Otherwise produce platform-neutral markdown.

## Success Metrics

- **Auto-adjudication rate:** target 60–80% of routine claims handled without human-engineer touch, with audit sampling
- **Average claim cycle time:** target under 72 hours from submission to disposition on in-warranty claims
- **Supplier-recovery rate:** target 60%+ of supplier-caused claim value recovered within one quarter of closure
- **Emerging-issue lead time:** target cluster detection at least 30 days ahead of field-campaign declaration
- **Fraud catch rate:** anomaly flags reviewed within five business days, with a tracked denial-on-anomaly rate (should be low — signals route to review, not denial)
- **Reserve accuracy:** actual-vs-accrued variance within ±10% at quarter close on the top five part families
