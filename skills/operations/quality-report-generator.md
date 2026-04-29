---
name: "Quality Report Generator"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~25 min/report"
version: 3.0
last_eval_score: 8.7
---

# Quality Report Generator

## Purpose

Turn inspection data (incoming, in-process, final, and customer returns) into a structured weekly / monthly quality report with defect Pareto, SPC trend read-out, Cp / Cpk interpretation, escape analysis, customer-CSR scorecard alignment (Ford Q1, GM BIQS, Stellantis CQI, Boeing AS13100, Lockheed AQS, Honeywell HOS, Caterpillar QPN, JLG, John Deere AQS, Toyota STR, GE Aviation S-1000D, FDA-listed program, etc.), APQP / PPAP gate framing where applicable, and prioritized corrective actions ranked by expected COPQ recovery — ready for a weekly quality review, a customer scorecard submission, or a management scorecard.

The skill is written against the bar that a customer SQE pulling a scorecard, an external auditor (TÜV, BSI, NSF-IBR, DEKRA, Lloyd's, NQA, DNV, AS9100 / IATF / ISO 13485 / FSSC 22000) reading the report as objective evidence, or a registrar reviewing §9.1.3 / §9.1 management-review-input quality data should not find a quote-worthy mistake in it.

## When to Use

Use for recurring quality reporting (daily, weekly, or monthly) or event-driven reporting (customer return, warranty spike, supplier quality issue, audit finding, NCR cluster, PPAP / FAI / ISIR submission, APQP gate review, Layered Process Audit cadence, customer scorecard window). Works best when inspection counts, defect classifications, SPC pulls, and a comparison baseline are available.

## Required Input

1. **Reporting period** — Start date, end date, scope (line, product family, supplier, customer, or plant-wide).
2. **Inspection data** — Counts from each inspection point:
   - Incoming (IQC / receiving): lots received, lots accepted, lots rejected, top supplier defects, supplier PPM.
   - In-process (IPQC / IP): units inspected, defects caught, defect class breakdown, by line / cell / shift.
   - Final (FQC / OQC / final-buy-off): units inspected, defects, FTQ %, escapes to customer if known.
   - Customer returns / RMA / 8D-active: units returned, complaint classes, disposition, days-to-disposition, days-to-8D-D3 / D4 / D5 / D8.
3. **Defect data** — Defect code, description, count, severity (critical / major / minor — per AQL or per customer CSR), inspection point where caught, KPC / KCC / "[Δ]" flag if applicable.
4. **Process / SPC data** — Control-chart read-out: any out-of-control points (Western Electric / Nelson rules), shifts, trends, stratification, autocorrelation. Cp / Cpk for CTQ features where capability is tracked. Note the underlying distribution (normal / non-normal); do not quote Cpk on non-normal data without a capability-of-non-normal-process method (Box-Cox, Johnson, percentile-based) and say so.
5. **Baseline for comparison** — Prior-period numbers, customer-specific PPM target, internal FTQ / DPMO target, COPQ target as % of revenue.
6. **Customer-CSR scorecard targets** — Per-customer PPM / OTD / PPAP / RMA-response / chargeback targets and current standing (Ford Q1 status, GM BIQS level, Stellantis CQI category, Boeing PMRT, Lockheed Gold / Silver / Bronze, Honeywell HOS supplier rating, Caterpillar QPN scorecard band, JLG SQ tier, etc.).
7. **APQP / PPAP / FAI status** — Any program in pre-launch with open APQP gates, any PPAP submission in window, any FAI / ISIR pending or rejected. A program in APQP-gate review needs a different framing than one at full production.
8. **Context** — New tooling, new operator, material change, design revision, supplier swap, seasonal factor, recently closed CAPA whose effectiveness window is open.
9. **COPQ data if available** — Internal failure cost (scrap, rework, sort), external failure cost (warranty, returns, chargebacks, premium freight), appraisal cost (inspection labor + gauge + capability), prevention cost (PPAP, training, FMEA / control-plan time). If unavailable, use scrap and rework as proxies and label as such.
10. **Audience** — Shop floor, plant leadership, customer quality team, corporate scorecard, registrar / external auditor (tone and depth adjust accordingly).

## Instructions

You are a manufacturing quality engineer or quality manager producing a defensible, decision-ready quality report. Your job is to translate raw inspection data into a narrative an operations leader can act on within 10 minutes — and that a customer SQE, external auditor, or registrar can read as objective evidence.

**Before you start:**

- Load `config.yml` for company / plant, lines / cells, customer-CSR list with current standing and targets (`quality.customer_csr.<customer>` — PPM target, OTD target, scorecard band, chargeback rate), QMS framework registered (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / 21 CFR 820 / 21 CFR 211 / FSSC 22000 / SQF / BRCGS / NADCAP / API Q1 / API Spec Q2), QMS platform in use (Plex, ETQ Reliance, MasterControl, Greenlight Guru, Sparta TrackWise, IQS, Veeva Vault QMS, AssurX, Maximo Quality, IBM Engineering Workflow, Pilgrim SmartSolve), SPC platform (InfinityQS ProFicient / SafetyChain, Minitab Real-Time SPC, JMP, Hertzler GainSeeker, Northwest Analytics, IntelliSPC), PPAP / FAI / ISIR cadence, internal FTQ / DPMO / COPQ targets.
- Reference `knowledge-base/regulations/` for the binding standards and customer CSRs (IATF 16949 §9.1.3 / §10.2.3, AS9100D §9.1.3 / §10.2, ISO 13485 §8.2 / §8.4 / §8.5, ISO 9001 §9.1.3, 21 CFR 820.250 / 820.100, 21 CFR 211.180 / 211.192, FSSC 22000 §9.1, AIAG-VDA FMEA, AIAG PPAP 4th edition, AS9102 Rev C FAI, customer-specific reference manuals — Ford CQDF / Q1 MS-9000, GM BIQS, Stellantis CQI-9 / -11 / -12 / -23 process audits, Boeing AS13100 / AQS / D6-82479, Lockheed AQS / SQAR, Honeywell SPOC-001, Caterpillar 1E1058 / SQEP, NADCAP audit checklists AC7004 / AC7101 / AC7108).
- Reference `knowledge-base/terminology/` for FTQ, DPMO, PPM, Cp, Cpk, Pp, Ppk, AQL, NCR, MRB, CAR, 8D, Six Sigma, COPQ, KPC, KCC, PSW, PPAP, FAI, ISIR, APQP, AIAG-VDA FMEA RPN / AP, control plan, layered-process-audit (LPA).

**Process:**

1. **Compute top-line metrics** for the reporting period:
   - FTQ (First Time Quality) % = good units ÷ total units inspected, by line / by stage.
   - DPMO or PPM at scale-appropriate level — externally to the customer (parts shipped basis), internally for IPQC (defects per unit basis). Be clear which.
   - Acceptance rate at IQC (lots), IPQC (units), FQC (units), OQC (units).
   - Customer PPM / complaint rate / RMA rate by customer if RMA data is provided.
   - Days-to-D3 (containment), Days-to-D5 (root cause), Days-to-D8 (closure) for any active 8D — most customer CSRs have a 24-hour D3 clock and a 30-day D8 clock.
2. **Build a defect Pareto by inspection point and customer**:
   - Rank defect classes by count AND by COPQ impact (top contributors may differ — a low-count critical defect can dominate COPQ).
   - Identify the vital few (top 3–5 classes accounting for ~80% of defects).
   - Cut by customer for any customer at >5% of period volume — customer-specific Paretos drive customer-specific actions.
3. **Read out SPC data** where provided:
   - List any out-of-control points (Western Electric / Nelson rule triggers — name the rule, not just "out of control").
   - Distinguish Cp (potential capability) from Cpk (actual, accounting for centering); Pp / Ppk for short-term study data.
   - Flag any CTQ feature with Cpk < 1.33 (general manufacturing standard), < 1.50 (some auto / aero programs), < 1.67 (safety / regulated / KPC).
   - Test the underlying distribution: if non-normal, do not quote Cpk without a non-normal capability method — use percentile-based or transformation, name the method.
   - Call out drift, shift, stratification, autocorrelation. Autocorrelated data on an X-bar / R chart inflates false alarms — flag the chart-design issue.
4. **Customer-CSR scorecard alignment.** For each named customer, compute against their scorecard:
   - Ford Q1 — PPM, OTD, PSO closure rate, MS-9000 audit status, current Q1 standing.
   - GM BIQS — current BIQS level (1–5), open BIQS elements, open SPPS / GP-12 actions.
   - Stellantis CQI — CQI-9 / CQI-11 / CQI-12 / CQI-23 audit status, AIAG PPAP element compliance, eSupplier scorecard rating.
   - Boeing — AS13100 / AS13003 conformance, PMRT supplier rating, AQS deliveries, escape rate, NMRR severity / cadence.
   - Lockheed — AQS standing (Gold / Silver / Bronze), SQAR maturity, in-process verification activity rate.
   - Honeywell HOS — supplier rating tier, SPOC-001 compliance, PPM, OTD, escape rate.
   - Caterpillar QPN — scorecard band (Bronze / Silver / Gold / Platinum), 1E1058 conformance.
   - Generic CSR — PPM target, OTD, RMA-response SLA, chargeback rate, scorecard band, last audit finding closure status.
   Mark any customer whose scorecard slipped a band or who has an open chargeback.
5. **APQP / PPAP / FAI gate framing** for any program in pre-launch:
   - Open APQP gates and percent of element completion (gate 1 plan / gate 2 design / gate 3 process / gate 4 product-validation / gate 5 launch / gate 6 lessons-learned).
   - PPAP element status: design records, AIAG-VDA DFMEA / PFMEA, control plan, MSA, dimensional / performance / appearance, capability studies, qualified-laboratory documentation, sample, master-sample, checking aids, customer-specific requirements, PSW. Name any element rejected on prior submission.
   - FAI / AS9102 status: Form 1 / 2 / 3 completeness, ballooned-print verification, any deltas from print to FAI.
   - ISIR (automotive sample) status if applicable.
6. **Trend analysis**:
   - Compare current period to baseline (prior period, target, benchmark).
   - Flag any defect class that rose >20% period-over-period.
   - Note any emerging stratification by shift, operator-cohort (not named), machine, material lot, supplier lot, tooling life-stage.
7. **Escape analysis** — for any customer return:
   - Identify the inspection point that should have caught the defect and why it didn't (containment gap, gauge R&R failure, sampling plan miss, KPC not flagged, AQL set too loose).
   - Quantify the COPQ — internal scrap / rework + external warranty + chargeback + premium-freight + customer-engineer-on-site cost.
   - Flag whether the escape triggers an automatic CAPA, a supplier 8D, a layered-process-audit re-cadence, or a customer-side controlled-shipping (CS-1 / CS-2 / Bayonet-2 / NSPL).
8. **COPQ summary** (or scrap+rework proxy if COPQ is not tracked):
   - Internal failure: scrap, rework, sort, downgrade.
   - External failure: warranty, returns, chargebacks, premium freight, CS-1 / CS-2 sort costs, recall.
   - Appraisal: inspection labor, gauge, capability studies.
   - Prevention: PPAP, training, FMEA / control-plan, LPA.
   - Total COPQ as % of revenue (target: <2% world-class, <4% acceptable, >5% emergency).
9. **Recommendations** — for the top 3–5 defect contributors and any escape:
   - Name the next action: containment, CAR / CAPA trigger, SPC addition, poka-yoke / error-proofing, training, supplier engagement (8D / SCAR), control-plan revision, PFMEA RPN re-evaluation, gauge R&R re-study, sampling-plan tighten, layered-process-audit re-cadence.
   - Rank by expected PPM / FTQ / COPQ recovery.
   - Tie to TPM / IATF / AS9100 clause and customer CSR — every major recommendation should cite the standard and CSR it strengthens.
10. **Escalations** — flag any item requiring immediate management, customer, or regulatory notification:
    - Safety-critical defect.
    - Recall trigger (FDA 21 CFR Part 7 / NHTSA 49 CFR 573 / CPSC 16 CFR 1115 / EU GPSD).
    - Customer-controlled-shipping trigger (CS-1, CS-2, Bayonet-2, NSPL, GP-12 ramp).
    - Adverse-event reporting (FDA MDR 21 CFR 803, EU MDR vigilance).
    - PSW invalidation / PPAP submission rejection.
    - Class I / Class II recall risk per the customer's own classification.

**Required output structure:**

```
QUALITY REPORT — [Period]                                      QMS: [framework + registrar]
Scope: [line / product / plant]            Audience: [shop floor / leadership / customer SQE / registrar]
Prepared by: [from config]                 Date: [generated date]
Customers in scope: [list with scorecard band]

EXECUTIVE SUMMARY
- FTQ: [%] (target [%], vs prior [%])
- DPMO / PPM: [value] (target [value])
- Escapes to customer: [count / PPM by customer]
- Top defect driver: [defect class] accounting for [%] of rejects ([%] of COPQ)
- Customer-CSR scorecard movement: [list any band / level slips]
- APQP / PPAP open: [count and status]
- COPQ: [% of revenue] ([direction vs prior])
- Bottom line in one sentence: [what leadership / SQE needs to know]

INSPECTION RESULTS BY STAGE
| Stage | Units / Lots Inspected | Defects | Accept Rate | Top Defect Class | Customer impact |
|-------|------------------------|---------|-------------|------------------|-----------------|
| IQC   | [count] lots           | [count] | [%]         | [class]          | [supplier PPM]  |
| IPQC  | [count] units          | [count] | [%]         | [class]          | [internal]      |
| FQC   | [count] units          | [count] | [%]         | [class]          | [escape risk]   |
| OQC   | [count] units          | [count] | [%]         | [class]          | [customer]      |
| RMA   | [count]                | N/A     | N/A         | [class]          | [PPM, days-to-D3] |

DEFECT PARETO (top contributors by count AND by COPQ)
| Rank | Defect Class | Count | % of Total | Cumulative % | Severity | KPC/KCC | COPQ Rank |
|------|--------------|-------|------------|--------------|----------|---------|-----------|
| 1    | [class]      | [n]   | [%]        | [%]          | [crit/maj/min] | [Y/N] | [rank] |

CUSTOMER-CSR SCORECARD ALIGNMENT
| Customer | Scorecard Band | PPM (period) | Target | OTD | RMA-resp | Open chargeback | Open audit findings |
| Ford     | Q1 [Y/N]       | [n]          | [n]    | [%] | [%]      | [$ / count]     | [count, severity]   |
| GM       | BIQS [level]   | [n]          | [n]    | [%] | [%]      | [...]           | [...]               |
| Stellantis | CQI [band]   | [n]          | [n]    | [%] | [%]      | [...]           | [...]               |
| Boeing   | PMRT [level]   | [n]          | [n]    | [%] | [%]      | [...]           | [...]               |
| ...      |                |              |        |     |          |                 |                     |

APQP / PPAP / FAI STATUS (programs in pre-launch only)
| Program | Customer | APQP gate | % complete | Open elements | PPAP submission window | FAI Form 1/2/3 |
| ...     | ...      | ...       | ...         | ...           | ...                    | ...             |

SPC / PROCESS-CAPABILITY READ-OUT
- Out-of-control points: [list with feature, chart type, rule violated]
- Distribution check on Cpk-quoted features: [normal / non-normal — method used]
- Cp / Cpk by CTQ feature:
  | Feature | Cp | Cpk | Pp | Ppk | Spec | Distribution | Method | Status |
  | ...     |    |     |    |     |      |              |        |        |
- Interpretation: [which features are capable, marginal, not capable]
- Autocorrelation / chart-design call-outs: [list]

TREND vs BASELINE
- FTQ: [trend direction, magnitude]
- Rising defect classes: [list with % change]
- Declining defect classes: [list]
- Stratification observed: [shift / cohort / machine / material / tooling life-stage — no named operators]

ESCAPE ANALYSIS (if customer returns present)
| RMA / 8D | Customer | Defect | Should-have-caught at | Containment-gap reason | COPQ | CSR trigger |
| ...      | ...      | ...    | ...                   | ...                    | $... | CS-1 / 8D / SCAR / GP-12 |

COPQ SUMMARY
| Bucket            | Period | % of Revenue | Δ vs prior | Notes |
| Internal failure  | $      | %            | ±          |       |
| External failure  | $      | %            | ±          |       |
| Appraisal         | $      | %            | ±          |       |
| Prevention        | $      | %            | ±          |       |
| Total COPQ        | $      | %            | ±          |       |

RECOMMENDED ACTIONS (ranked by expected COPQ / PPM impact, max 5)
| # | Action | Target Defect Class | Owner Type | Expected Impact (PPM / FTQ / $) | Trigger | Standard / CSR strengthened |
|---|--------|---------------------|------------|---------------------------------|---------|----------------------------|
| 1 | [action] | [class]           | [QE/Eng/Prod/Supplier] | [PPM, FTQ, $] | [CAR / CAPA / SPC / Training / Supplier 8D] | [IATF §X / AS9100 §Y / Ford Q1 / GM BIQS] |

ESCALATIONS
- [Any items requiring management, customer, or regulatory notification — name the clock and the deliverable]

DATA QUALITY NOTES
- [Limitations: missing defect codes, small sample, gauge R&R issues, AQL appropriateness, distribution assumptions, etc.]
```

**Output requirements:**

- Every metric carries a target or baseline comparison. A number alone is not a finding.
- Defect Pareto uses both count AND severity AND COPQ rank — critical defects with low counts still get called out.
- Cp / Cpk interpretation is explicit (capable / marginal / not capable), tied to customer expectations (1.33 standard / 1.50 some programs / 1.67 safety / KPC), and conditioned on a stated distribution check. No Cpk on non-normal data without the method named.
- Customer-CSR scorecard rows appear for every named customer at >5% of period volume. Slipped bands and open chargebacks are escalated.
- Escape analysis is honest: if containment failed, name the inspection point and the gap mechanism.
- Recommendations are ranked by expected impact, name the standard / CSR they strengthen, and never exceed 5.
- COPQ frame uses internal / external / appraisal / prevention buckets; if true COPQ is not tracked, use scrap+rework as a proxy and label as such.
- No hedge words. State findings as facts; state hypotheses explicitly as hypotheses.
- Round percentages to 1 decimal, PPM / DPMO to whole numbers, COPQ $ to nearest $1K.
- If data is missing for a section, include the section header with "[insufficient data — see Data Quality Notes]" rather than omitting it.
- Saved to `outputs/` if the user confirms; for QMS-platform export, render the platform-native structured doc per `config.yml`.

## Anti-Patterns to Avoid

- **Do not** quote Cpk on non-normal data without naming the non-normal capability method (Box-Cox, Johnson, percentile-based). Doing so is the most common reason a customer-side SQE bounces a quality report back as inadmissible.
- **Do not** mix internal DPU-basis DPMO with external parts-shipped-basis customer PPM in the same row. Different denominators, different stakes — call them out separately.
- **Do not** characterize a named operator's performance in the report. The report is on the system, not the worker. "Stratification by shift cohort" is fine; "Operator J. Doe drove most of the rejects" is not.
- **Do not** claim root cause from inspection data alone. Inspection data points to hypotheses; PFMEA, gauge R&R, and floor verification confirm cause.
- **Do not** skip the customer-CSR scorecard alignment for any named customer at >5% of period volume. A weekly quality report that does not state where the plant stands on Ford Q1 / GM BIQS / Stellantis CQI / Boeing PMRT / Lockheed AQS / Honeywell HOS / Cat QPN is not a customer-facing report.
- **Do not** invent PPAP / FAI / ISIR element status — if the QMS platform doesn't expose it, mark "[unknown — pending QMS pull]" and route the question to the program manager.
- **Do not** forget to test the SPC chart's design assumptions before quoting alarms. Autocorrelation, mixture, and trend on an X-bar / R inflate false alarms; cite the chart-design issue rather than chasing the alarm.
- **Do not** treat AQL acceptance as evidence the lot is good. AQL is a risk balance — quote the rejection number and the consumer-risk LTPD too.
- **Do not** omit COPQ. If true COPQ is not tracked, use scrap+rework as a proxy and label it. Operations leaders need a $ frame, not a PPM frame, to prioritize.
- **Do not** skip the containment-gap mechanism on an escape. "QC missed it" is not a mechanism. Name whether it was a sampling plan, a gauge R&R failure, a missing KPC flag, or a stale control plan.
- **Do not** issue the report without the standard / CSR each recommendation strengthens. A recommendation that doesn't strengthen a clause / CSR is decoration.
- **Do not** drop the data-quality block. The block is what makes the report admissible to a registrar; without it, every alarm is contestable.

## Integration Notes

- **Pairs with CAPA Document Builder** — every recommendation that crosses the CAR / CAPA threshold seeds a CAPA. The verification metric in the CAPA must match the metric in this report.
- **Pairs with Compliance Audit Prep** — the QMS-clause cross-reference here is the same clause set Compliance Audit Prep walks. A weekly quality report is the §9.1.3 / §9.1 management-review-input record-of-record.
- **Pairs with Vision Inspection Summary** — vision-system defect categories and false-reject rates flow into this report's defect Pareto and escape analysis.
- **Pairs with Supplier Communication Drafter** — supplier 8D / SCAR / containment-letter triggers are surfaced here and drafted there.
- **Pairs with Warranty Claim Analyzer** — external-failure COPQ in this report is the input to the warranty analyzer's recurring-mode pattern detection.
- **Pairs with Tariff Impact Analysis** — supplier swaps under tariff pressure show up as Material-cohort variation in the SPC and Pareto data; cross-check before attributing to operator or method.
- **Pairs with Production Scheduling Optimizer** — Cpk < 1.33 features should constrain the scheduler's variant assignment until a process-capability action closes.
- **Pairs with OEE Analysis Report and Downtime Analysis Summary** — performance-loss and availability-loss data is the partner data set; treat together, not separately.

## Success Metrics

- **Customer PPM trend by named customer** (target: meet or beat each customer-CSR target).
- **Customer-CSR scorecard band stability** (target: no band slip period over period).
- **Escape rate** (target: trending toward zero on KPC / KCC / safety-critical).
- **8D D3 / D5 / D8 cycle times** (target: meet each customer's contractual clock — typical 24-h D3, 14-d D5, 30-d D8).
- **CAR / CAPA effectiveness rate** (target: ≥85% effective at the verification window; an "effective" CAPA whose metric didn't move is failed).
- **PPAP / FAI submission first-pass rate** (target: ≥95%; rejected PPAP elements are a leading indicator of an APQP gate that closed too early).
- **Cpk distribution-check discipline** (target: 100% of Cpk-quoted features have the underlying distribution stated).
- **COPQ as % of revenue** (target: trending down; world-class <2%, acceptable <4%, emergency >5%).
- **Audit-finding rate** on §9.1.3 / §9.1 / §10.2 management-review and corrective-action clauses (target: zero major, ≤2 minor per surveillance audit).

## Example Output

> [Reference example will be populated by the eval system. For now, run the skill with sample input to see output quality.]
