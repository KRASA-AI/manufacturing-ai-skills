---
name: "CAPA Document Builder"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~75 min/CAPA"
version: 4.0
last_eval_score: 9.0
---

# CAPA Document Builder

## Purpose

Build an audit-ready Corrective and Preventive Action record that satisfies ISO 9001 §10.2, IATF 16949 §10.2.3–10.2.6 (problem-solving / error-proofing / lessons-learned), AS9100D §10.2, FDA 21 CFR 820.100 (medical devices), 21 CFR 211.192 (pharma deviation/CAPA), and the customer-specific 8D requirements of every major automotive and aerospace OEM CSR — with an IS / IS-NOT problem statement, KPC/KCC-aware root-cause analysis tied to the control plan and PFMEA, separated containment / corrective / preventive actions, structured agentic-RCA-pattern output, a defined effectiveness-verification plan with sampling-plan rigor, an explicit controlled-document update list, and a QMS-platform-native record ready for upload to MasterControl / ETQ Reliance / Plex / Greenlight Guru / Sparta TrackWise / IQS / Veeva Vault QMS / AssurX / SAP QM.

The CAPA Document Builder is the corrective-action destination consumed by 7+ skills in this repo (Safety Incident Report, Compliance Audit Prep, Quality Report Generator, Supplier Communication Drafter, Vision Inspection Summary, SOP Writer, Work Instruction Generator, Warranty Claim Analyzer, OT Cyber Incident Response). It must match the depth of the upstream skills on quality-record, PFMEA, control-plan, and customer-CSR linkage.

## When to Use

Use this skill whenever a non-conformance, customer complaint, audit finding, internal-audit observation, safety-incident contributing factor, OT-cyber event, supplier 8D, or repeat escape requires a formal CAPA record. Specific triggers:

- Customer complaint, field return, or warranty escape with systemic implications
- NCR severity at major/critical, or ≥2 occurrences of the same failure mode in 12 months on the same part family
- Customer-flagged PPAP / capability problem, including any KPC/KCC PPM-commitment escape against an active customer CSR (Ford QOS / GM 1927-44 / Stellantis Q-CSR / Toyota CSL2 / Honda QAS / Nissan QAV / Tesla SQA / Boeing D6-83107 / Airbus AQP / Raytheon SCR)
- Supplier 8D coming back that requires a mirror CAPA on our side (incoming-inspection plan update, control-plan update, PPAP-impact assessment)
- Internal-audit NC or external-audit finding (3rd-party registrar, customer site audit, regulatory inspection)
- Safety-incident contributing factor escalated from the safety-incident-report skill (system-cause CAPA, not training-only)
- Repeat escape from vision inspection or end-of-line test (vision-inspection-summary cluster signal)
- OT cybersecurity incident with quality-record or product-impact (ot-cyber-incident-response handoff)
- Pharma 21 CFR 211.192 deviation or out-of-specification (OOS) investigation
- Medical-device 21 CFR 820.198 complaint requiring CAPA
- AS9100 §10.2 nonconformity, including ARP 5089-style escape-prevention triggers
- Recurring scrap on the same characteristic, line, or shift exceeding control-plan reaction-plan threshold

## Two-Pass Model

This skill runs in two passes so the record can be **opened in minutes and completed as the investigation matures**, instead of stalling because the full 13-section 8D apparatus demands ten input categories the moment a non-conformance lands.

- **Pass 1 — Quick CAPA Initiation (D0–D3).** Minimal input: the trigger, a factual problem description, and what was already done on the floor. Returns the controlled-document header, a first-cut IS/IS-NOT skeleton, the containment table, the KPC/KCC and customer-CSR notification clocks that *just started*, and a ranked Pass-2 gap list. Built to open the record and stop escapes **before** root cause is known. This is the pass to run during the first hour of a customer complaint or a major NCR.
- **Pass 2 — Full CAPA (D4–D8).** The complete 13-section record below: separated occurrence/detection root cause, matched corrective and preventive actions, read-across, the effectiveness-verification plan, the controlled-document update set, and closure. Run it as RCA data arrives — it inherits everything Pass 1 already captured.

**Use Pass 1 when** the clock is running and you need a defensible record open *now* (customer CSR containment-certification clocks are typically 24h notification / D3 in 3 days). **Skip to Pass 2 when** containment is already complete and root-cause data is in hand. Pass 1 is explicitly allowed to run partial — every unknown is marked `TO BE COMPLETED`, never fabricated — and is designed to be re-run as facts arrive.

### Pass 1 — Quick CAPA Initiation (run on three inputs)

Required to start: (1) **Trigger** (complaint / NCR / audit / supplier-8D / safety / vision-alarm / warranty-cluster / OT-cyber ID), (2) **Problem description** (what / where / when / how many / how detected — operator or customer verbatim preserved), (3) **Immediate actions already taken** (or "none yet").

Pass 1 returns, from config plus those three inputs only:

```
QUICK CAPA INITIATION — D0–D3

CAPA No:        [CAPA-YYYY-NNNN per config tools.qms doc numbering — provisional]
Title:          [failure mode + part family]
Status:         Open — Containment in progress
Severity (prov):[Critical / Major / Minor — from config.quality.defect_taxonomy severity if the class matches; else TO BE COMPLETED]
Customer CSR:   [matched from config.customer_csr_list by affected program → notification clock]
Trigger:        [...]            Opened: [date]   D3-due: [Opened + CSR containment window]

PROBLEM (IS / IS-NOT — first cut)
  As reported (verbatim): "[unchanged]"
  IS:     what / where (config.operations.line_cell_inventory) / when / how much / how detected
  IS NOT: [leave as TO BE COMPLETED if not yet verified — do not guess]

KPC/KCC FLAG: [if the affected characteristic maps to a config.quality.defect_taxonomy KPC ◆ / KCC ▲ class, surface it now — it drives the PPM-commitment clock]

CONTAINMENT (D3) — minimum set, expand in Pass 2
  | # | Action | Scope | Owner | Started | Evidence |
  (quarantine WIP · sort-and-certify outbound · 100% screen at detection point ·
   notify customer if CSR clock applies · secure stock at customer plant)

CLOCKS THAT JUST STARTED
  - Customer notification: [config.customer_csr_list clock for the matched program]
  - Regulatory (if FDA/NHTSA/FAA/OSHA in play): [statutory clock]
  - Internal D3 containment-certification: [date]

PASS-2 GAP LIST (what Full CAPA still needs)
  1. Occurrence + detection root cause (D4) — not started
  2. Matched corrective/preventive actions (D5–D7)
  3. Effectiveness-verification plan (metric/threshold/sample)
  4. Controlled-document update set (D7)
  5. Related-history pull (12–24 mo, same part family/process/supplier)
```

Five escalation triggers that mean **do not stop at Pass 1 — go to the full record promptly**: (a) any KPC/KCC characteristic or active customer-CSR PPM escape; (b) severity Major/Critical; (c) any regulatory notification clock (FDA / NHTSA / FAA / OSHA); (d) ≥2 occurrences of the same failure mode in 12 months; (e) a customer-driven 8D is owed. Absent all five, a minor internal NCR may legitimately close on a lightweight Pass-1-plus-RCA record.

## Config Pre-Population

Bind these `config.yml` keys so standing facts are never re-asked each CAPA. Where a key resolves, fill the field from config and mark it `[from config]`; where it does not, fall back to `TO BE COMPLETED`.

| Output field | Config key |
|---|---|
| CAPA numbering convention | `tools.qms` (e.g. ETQ Reliance) + doc-numbering convention |
| QMS export target | `tools.qms` → QMS-platform export format section |
| Applicable standards / framework default | `quality.certifications` (ISO 9001 / IATF 16949 / AS9100D …) → 8D default for IATF/AS customers |
| Characteristic KPC/KCC + severity | `quality.defect_taxonomy[].kpc_kcc` / `.severity` |
| Customer CSR + PPM commitment + notification clock | `customer_csr_list[]` (scorecard, ppm_commitment, portal) |
| Affected line/cell + criticality | `operations.line_cell_inventory[]` |
| Threshold / disposition owner | `quality.threshold_policy_owner` |
| Calibration band (measurement-integrity CAs) | `quality.calibration_band` |
| Supplier mirror-CAPA routing | `supply_chain.tiered_suppliers[]` (origin, single_source, criticality) |
| EHS approver (safety-linked) | `ehs.ehs_lead` |
| IT/OT handler (cyber-linked) | `cyber` block (retained_dfir, ot_monitoring_platform) |
| Operator requalification system | `workforce.training_records_system` |
| Voice / signature on customer-facing 8D | `voice.signature_block` |

For the example config (`Summit Precision`), a weld-porosity escape auto-resolves to: framework **8D** (IATF 16949 + AS9100D customers), characteristic **weld-porosity → KPC ◆, critical**, affected cell **WELD-1 (critical)**, customer **Ford Q1 (PPM commitment 15, Covisint/QPMT portal, 24h notification)**, QMS export **ETQ Reliance**, requalification via **ETQ Reliance training module** — none of which the user re-enters.

## Required Input

Provide the following. Mark any item unknown as "TO BE COMPLETED" — never fabricate.

1. **Trigger** — Complaint #, NCR #, audit finding #, supplier 8D #, safety incident #, vision-system alarm ID, warranty claim cluster ID, or OT-cyber event ID
2. **Problem description** — Factual: what was observed, where (line/cell from `config.yml → line_cell_inventory`), when, how many, how detected, who detected. Keep the customer's or operator's verbatim wording in a separate "as reported" field — do not rewrite it
3. **Specification / requirement not met** — Drawing number and revision, control-plan line item, PFMEA line item, customer CSR clause, regulatory clause, internal procedure document number — the "what should have happened" with citation, not paraphrase
4. **Severity, scope, and KPC/KCC linkage** — Units affected, lots affected, customers affected, financial exposure, regulatory exposure, safety exposure. **Explicitly mark whether any affected characteristic is a KPC (◆) or KCC (▲)** and which customer CSR PPM commitment is at risk
5. **Immediate actions already taken** — Anything done on the floor before CAPA was opened (quarantine, sort-and-certify, stop-ship, customer notification, regulatory notification)
6. **Related history (12–24 months)** — Prior NCRs, CAPAs, near-misses, warranty claims, safety incidents, supplier 8Ds involving the same part family, process, equipment, supplier, operator group, line, or characteristic
7. **Team** — CAPA owner, cross-functional team members (quality, engineering, production, supplier-quality if upstream, customer-contact if downstream, EHS if safety-linked, IT/OT if cyber-linked). Pull names from `config.yml → team` where available
8. **Standards applicable** — From `config.yml → qms_framework`: ISO 9001, IATF 16949, AS9100, ISO 13485 / 21 CFR 820, 21 CFR 210/211, AS9120, ITAR/EAR if defense, plus the relevant customer CSR list from `config.yml → customer_csr_list`
9. **Target close window** — Per customer CSR (Ford / GM / Stellantis / Toyota commonly 60 days for D5; 30 days for D3 containment certification), regulatory window (FDA 21 CFR 820.100 has no hard close window but expects timely with rationale; FDA 21 CFR 211.192 OOS investigations have a ~30-day target), AS9100 supplier escape investigation per Boeing D6-83107 (60 days), or ISO 9001 internal default (90 days)
10. **Problem-solving framework** — `8D` (automotive/supplier-facing default), `A3` (lean / internal continuous-improvement), `5-Why + fishbone` (internal events, lower-complexity), or `Shainin Red-X` (large-population statistical engineering). Default to 8D when trigger is a customer complaint or supplier issue

## Instructions

You are a manufacturing quality engineer writing a CAPA that will be read by an ISO / IATF / AS9100 / FDA auditor, by the customer's supplier-quality team, and by your own corporate quality leadership. Your job is to produce a record that (a) states the problem precisely, (b) separates symptom from root cause, (c) distinguishes occurrence-cause from detection-cause and containment-from-corrective-from-preventive, (d) links every characteristic to its KPC/KCC designation and control-plan reference, (e) commits to an effectiveness-verification test with a defined sample, threshold, and window, (f) lists every controlled document that must be updated for closure, and (g) never claims closure until the verification threshold is met.

**Before you start:**

- Load `config.yml` for: company name, OSHA establishment ID, QMS framework (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / 21 CFR 820 / 211), QMS platform name, QMS document numbering convention, customer CSR list (for KPC/KCC and PPM-commitment escape framing), supplier-tier matrix, line/cell inventory, asset registry, PFMEA reference set, control-plan reference set, severity-rating matrix, approval authority matrix, named CAPA-board reviewers, default reviewer and approver titles, EHS personnel for safety-linked CAPAs, IT/OT incident-handler for cyber-linked CAPAs, and the post-launch / regular-production lifecycle stage of the affected part family.
- Reference `knowledge-base/regulations/` for current clause language. Quote, do not paraphrase. Key references:
  - **ISO 9001:2015 §10.2** — Nonconformity and corrective action
  - **ISO 9001 FDIS / 2026** — clause numbers held; ISO 9001:2026 expected publication Q3 2026 with three-year transition; note where 2026 clause shifts may apply but cite the in-force standard
  - **IATF 16949:2016 §10.2.3** (problem-solving), **§10.2.4** (error-proofing), **§10.2.5** (warranty management), **§10.2.6** (customer complaints and field-failure test analysis)
  - **IATF 16949 §8.5.1.4** (control-plan update on corrective action), **§8.3.6** (design and development changes)
  - **AS9100D §10.2** — Nonconformity and corrective action
  - **AS9145 (APQP and PPAP for aerospace)** — escape-prevention and lessons-learned linkage
  - **FDA 21 CFR 820.100** — CAPA (medical devices), with FDA Quality Management System Regulation (QMSR) harmonization effective 2026-02-02 to align with ISO 13485:2016
  - **FDA 21 CFR 211.192** — Production record review and deviation investigation (pharma)
  - **FDA 21 CFR 210 / 211** — cGMP, with §211.180 record retention applicable to CAPA records
  - **AIAG CQI-20** — Effective problem-solving
  - **AIAG PPAP 4th ed. §2.2** — Customer notification and submission triggers (used in change-classification disposition)
  - **AIAG MSA 4th ed. §7** — Measurement system analysis (referenced in effectiveness-verification plan when measurement integrity is in question)
  - **AIAG & VDA SPC Manual (1st ed.)** — Statistical process control (referenced in capability-driven effectiveness verification; new edition gate date 2026-06-30 per AIAG XD-SPCDRAFT)
- Decide the problem-solving framework with the user. Default to **8D** when the trigger is a customer complaint or supplier issue. Use **5-Why + fishbone** for shorter internal events. Use **A3** for lean continuous-improvement events. Use **Shainin Red-X** for population-level statistical engineering (typically with Engineering involvement).

**Process:**

**Step 1 — Open the CAPA record with the controlled-document header.**

```
CORRECTIVE AND PREVENTIVE ACTION REPORT

CAPA No:        [CAPA-YYYY-NNNN per config.yml → doc_numbering]
Title:          [Specific failure mode + part family — searchable in QMS platform]
Status:         [Open / Containment Complete / RCA Complete / CA Implemented / Pending Verification / Closed-Effective / Closed-Not-Effective-Reopened]
Severity:       [Critical / Major / Minor — per config.yml → severity_matrix]
Standards:      [ISO 9001 §10.2 / IATF 16949 §10.2.3–10.2.6 / AS9100D §10.2 / 21 CFR 820.100 / 21 CFR 211.192 / customer CSR — select all applicable from config.yml]
Customer CSR:   [Ford QOS / GM 1927-44 / Stellantis Q-CSR / Toyota CSL2 / Honda QAS / Nissan QAV / Boeing D6-83107 / Airbus AQP / Raytheon SCR — list with clause reference for any CSR-driven element]
Framework:      [8D / A3 / 5-Why+Fishbone / Shainin Red-X]
Trigger:        [complaint # / NCR # / audit finding # / supplier 8D # / safety incident # / vision alarm # / warranty cluster # / OT cyber event #]
Received:       [date]   Opened: [date]   Target close: [date]   Actual close: [date or N/A]
Owner:          [Name, Title, Function]
Team:           [Quality Engineer / Process Engineer / Production / Supplier Quality / Customer Contact / EHS / IT-OT — list with names and functions]
```

**Step 2 — Write the problem statement in IS / IS-NOT format (8D D2 / A3 Background).**

```
2. PROBLEM STATEMENT (IS / IS-NOT)

As reported (verbatim): "[customer / operator / auditor wording, unchanged]"

IS:
  - What:        [specific defect / non-conformance — symptom in measurable terms]
  - Where:       [line, cell, station from config.yml → line_cell_inventory; or customer site / dealer / field location]
  - When:        [first observed; date range; frequency; per-shift breakdown if shift-correlated]
  - How much:    [quantity / PPM / Cpk delta / $ exposure / units in customer hand vs. in containment vs. scrapped]
  - How detected: [stage and method — customer / dealer / incoming / in-process / final / vision / SPC / EOL test / field service]

IS NOT:
  - Not affected: [parts / lines / shifts / dates / customers / lots that have been verified clear and the verification method used]
  - Not this similar-looking failure mode: [explicit differentiation from related defect codes]

Requirement (governing specification):
  - Drawing / spec:    [number, revision, characteristic ID]
  - Control plan:      [CP number, revision, line item]
  - PFMEA:             [PFMEA number, revision, line item, RPN before]
  - Customer CSR:      [CSR clause — quoted, not paraphrased]
  - Regulation:        [21 CFR § / ISO clause — quoted, not paraphrased]
```

**Step 3 — Mark KPC/KCC and PPM-commitment exposure.**

```
3. KPC / KCC AND CUSTOMER PPM-COMMITMENT EXPOSURE

Characteristic affected:           [specific dimension / parameter / feature]
KPC ◆ / KCC ▲ / Standard ●:        [designation per control plan / PFMEA]
Drawing / control-plan symbol:     [symbol and reference where designated]
Customer CSR PPM commitment:       [from config.yml → customer_csr_list; e.g., Ford QOS PPM target 25, GM PRR-2 entry trigger, Toyota CSL2 target]
PPM-commitment impact estimate:    [escape × population, with confidence and method]
Controlled-shipping trigger?       [Yes — Ford CSL-1 / CSL-2 / GM CS-I / CS-II / Toyota CL2 / CL3 — with notification clock; or No]
Stop-ship at customer plant?       [Yes / No — and customer plant identifier]
Field action / recall implication: [None / Pending review / TSB / Field campaign / Safety recall — with regulatory authority if applicable]
```

**Step 4 — Open containment immediately (8D D3).**

Containment is not corrective action. Hold the line while root cause is being found.

```
4. CONTAINMENT (D3)

| # | Action                              | Scope (lots / stations / qty) | Owner          | Date started | Date complete | Evidence (tag # / cert / record) |
|---|-------------------------------------|-------------------------------|----------------|--------------|---------------|----------------------------------|
| 1 | Quarantine WIP lot [###]            | [qty]                         | [name]         | [date]       | [date]        | [hold tag #]                     |
| 2 | Sort-and-certify outbound at [stage]| [qty / stations]              | [name]         | [date]       | [date]        | [sort record # / cert]           |
| 3 | 100% inspection added at [point]    | [stations affected]           | [name]         | [date]       | [date]        | [QMS work-instruction rev]       |
| 4 | Customer notified (CSL/CS-I if req) | [customer, plant, contact]    | [supplier QE]  | [date]       | [date]        | [email / portal ref]             |
| 5 | Stock at customer plant secured     | [qty at risk]                 | [customer SQA] | [date]       | [date]        | [customer hold tag]              |
| 6 | Field stock (dealer / service)      | [qty estimated, by region]    | [warranty lead]| [date]       | [date]        | [TSB or service bulletin #]      |

Containment verified by: [name, title, date]
Containment effectiveness check: [zero escapes since containment date at the detection point, confirmed by inspection / SPC data over the verification window]
```

**Step 5 — Root cause analysis (8D D4).**

Separate occurrence root cause (why it happened) from detection root cause (why it escaped). Both need their own corrective actions.

```
5. ROOT CAUSE ANALYSIS (D4)

5.1 OCCURRENCE ROOT CAUSE
    5-Why (or extended chain — keep going until you reach a system cause):

    1. Why was the defect produced? → [trigger]
       Evidence: [data / measurement / interview / process video]                 Verified ☐  Inferred ☐
    2. Why did that happen? → [mechanism]
       Evidence: [...]                                                            Verified ☐  Inferred ☐
    3. Why did that happen? → [underlying condition]
       Evidence: [...]                                                            Verified ☐  Inferred ☐
    4. Why has the condition persisted? → [process / method gap]
       Evidence: [...]                                                            Verified ☐  Inferred ☐
    5. Why does the system allow it? → [governance / design / control plan gap]
       Evidence: [...]                                                            Verified ☐  Inferred ☐

    Parallel technique (required — single-thread bias check):
    [Fishbone 4M+E / fault-tree / is-is-not / Shainin component-search / DOE result]
    Result: [convergence on the same root cause? or divergence requiring further investigation?]

    Occurrence root cause (one sentence): [statement]
    Classification: [Method / Machine / Material / Manpower / Measurement / Environment]
    Confidence: [high / medium / low]   What would raise confidence: [specific test / data / replication]

5.2 DETECTION ROOT CAUSE
    Why was this not caught at [incoming / in-process / SPC / vision / EOL test / LPA / audit]?

    1. Why was the existing detection inadequate? → [gauge / sample plan / threshold / training / coverage gap]
    2. Why was the detection method specified that way? → [PFMEA detection-rating assumption / control-plan design]
    3. Why was the detection method not validated against this failure mode? → [MSA / capability / coverage assumption]
    4. Why did the audit / LPA not surface the gap? → [LPA question set / sampling frequency]

    Detection root cause (one sentence): [statement]
    PFMEA detection-rating reassessment: [current rating vs. revised rating per AIAG-VDA FMEA Handbook]
    PFMEA RPN before / after: [number → number]

5.3 EVIDENCE BASE
    [List with source — test report #, measurement data set, process video timestamp, interview record, layered-process-audit observation, SPC chart export, vision-system event log, MES quality-event extract]

5.4 AGENTIC-RCA STRUCTURED FIELDS (if RCA was performed with or assisted by an agentic AI tool such as Augmentir Augie's 5 Why Coach / Root Cause Investigator / Data Analyst, Microsoft Manufacturing Inspector Agent, Siemens Eigen Engineering Agent, or SAP Production Planning and Operations Agent)
    - Agentic tool used:               [name / version]
    - Human-in-the-loop owner:         [name, title]    Sign-off date: [date]
    - AI-generated 5-Why output:       [attached / referenced]
    - Human-validated 5-Why output:    [attached / final version — divergences from AI output listed]
    - Statistical analysis output:     [data sets, methods, results — replicable by hand]
    - AI-flagged candidate causes:     [list with disposition: accepted / rejected / requires further investigation]
    Note: Agentic-RCA structured output supplements but does not replace the cross-functional team's review. Every "why" still carries Verified / Inferred status and the human owner signs off the final RCA.
```

**Step 6 — Write corrective actions that match the root cause (8D D5 + D6).**

```
6. CORRECTIVE ACTION (D5 — chosen; D6 — implemented)

For OCCURRENCE root cause:
| # | Action                                  | Type                                                      | Owner | Due  | Status | Verification of implementation |
|---|-----------------------------------------|-----------------------------------------------------------|-------|------|--------|-------------------------------|
| 1 | [specific action mapped to root cause]  | [SOP rev / PFMEA rev / Control Plan rev / ECR / PM / poka-yoke / DOE / equipment-cap upgrade / supplier ECR / training-and-requalification] | [name]| [date]| [status]| [evidence reference]          |

For DETECTION root cause:
| # | Action                                  | Type                                                      | Owner | Due  | Status | Verification of implementation |
|---|-----------------------------------------|-----------------------------------------------------------|-------|------|--------|-------------------------------|
| 1 | [specific action — gauge / sample plan / threshold / vision-system retrain / LPA addition] | [Control Plan rev / Vision-system threshold / LPA question / Gauge R&R re-validation] | [name]| [date]| [status]| [evidence reference]          |

Match-to-root-cause rule (apply per action):
  - Method root cause          → SOP / control-plan / PFMEA update + operator requalification
  - Equipment root cause       → PM interval change / calibration frequency / engineering change / capability upgrade
  - Material root cause        → supplier CAPA (see Supplier Communication Drafter `scar-8d-request`) + incoming-inspection update + supplier-PPAP review
  - Training root cause        → curriculum revision + qualification recheck through Training Plan & Skill Matrix (LMS record); never "retrained operator" alone
  - Design / spec root cause   → ECR with drawing revision and PPAP impact assessment per AIAG PPAP §2.2
  - Measurement root cause     → MSA per AIAG MSA 4th ed. §7 (effective resolution / linearity / repeatability / reproducibility); replace or recalibrate gauge as needed
  - Detection root cause       → control-plan inspection point / vision threshold / gauge / LPA question; update PFMEA detection rating
  - Governance / system cause  → procedure update + management-review agenda item + LPA question addition
```

**Step 7 — Write preventive / read-across actions (8D D7).**

A CAPA without read-across usually repeats in a different form. Search every dimension explicitly.

```
7. PREVENTIVE ACTION / READ-ACROSS (D7)

Same failure mode searched across:
  - Other part numbers in family:                    [list checked; result; CAPA opened if needed]
  - Other lines using same process or equipment:     [list checked; result]
  - Other shifts running the same operation:         [list checked; result]
  - Other suppliers providing same material / process:[list checked; result; supplier CAPA opened if needed]
  - Other customers / programs potentially affected: [list checked; notification status per CSR]
  - Other part families with the same characteristic / design pattern: [list checked; result]
  - Historical CAPAs / NCRs with same failure mode:  [search window months; matches found; pattern analysis]
  - Warranty claim cluster on same part family:      [reference Warranty Claim Analyzer cluster ID if any]
  - Field-action / TSB / recall implication:         [None / Pending / Open]

Read-across CAPAs opened:                            [list with CAPA #s and target close dates]
Lessons-learned record:                              [LL record # / location — per IATF 16949 §10.2.6 and AS9100 §10.2 lessons-learned requirement]
Error-proofing (poka-yoke) opportunity assessed:     [Yes — implemented / Yes — deferred with rationale / Not applicable — per IATF 16949 §10.2.4]
PFMEA library update:                                [yes — RPN reduction documented / no — rationale]
```

**Step 8 — Define the effectiveness-verification plan (8D pre-D8) before closure, not after.**

```
8. EFFECTIVENESS VERIFICATION PLAN

Metric:                  [what will be measured — defect rate / PPM / Cpk / escape count / SPC stability]
Threshold:               [pass criterion — e.g., zero escapes / ≤ 25 PPM / Cpk ≥ 1.33 / SPC stable per Western Electric / Nelson rules]
Sample plan:             [sample size / number of consecutive lots / calendar window / units produced]
                         Statistical basis: [ANSI/ASQ Z1.4 reference / consumer-risk and producer-risk levels / power]
Measurement system:      [gauge / vision / SPC platform — MSA validated per AIAG MSA 4th ed. §7; Gauge R&R % within tolerance]
Who measures:            [function, named owner]
Cadence:                 [daily / weekly / per-lot]
Review owner:            [CAPA board reviewer per config.yml → capa_board]
Review meeting:          [daily quality stand-up / weekly CAPA review / monthly QMR — name and cadence]
Records location:        [QMS platform record # / SPC platform export ID / shared drive path]
Customer / supplier signoff: [customer SQA acceptance criteria if customer-facing CAPA; supplier 8D D8 sign-off if supplier-driven]

Closure is contingent on meeting the threshold for the full sample window.
Do NOT close on corrective-action-implemented alone. Closure-without-verification is a top-3 audit finding.

Re-open trigger: [recurrence within 12 months / capability regression / customer escape — explicit re-open conditions and CAPA-board override authority]
```

**Step 9 — Update the controlled-document set (8D D7 continued).**

Missing document updates is a top-3 audit finding against "closed" CAPAs. Every CAPA must close this loop explicitly.

```
9. CONTROLLED-DOCUMENT AND SYSTEM UPDATES

| Document type                                    | Document # / ID | Revision before → after | Effective date | Updated by | Verified |
|--------------------------------------------------|-----------------|--------------------------|----------------|------------|----------|
| Control plan                                     | [CP-####]       | [Rev → Rev]              | [date]         | [name]     | ☐        |
| PFMEA                                            | [PFMEA-####]    | [Rev → Rev; RPN ## → ##] | [date]         | [name]     | ☐        |
| Process flow diagram                             | [PFD-####]      | [Rev → Rev]              | [date]         | [name]     | ☐        |
| SOP                                              | [SOP-####]      | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Work Instruction / DWI                           | [WI-####]       | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Inspection / test method                         | [TM-####]       | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Vision-system threshold or model                 | [model hash]    | [version → version]      | [date]         | [name]     | ☐        |
| LPA question set                                 | [LPA-####]      | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Training record / requalification (LMS)          | [LMS ticket #]  | [Operators requalified: #]| [date]        | [name]     | ☐        |
| PPAP / Customer notification (per AIAG PPAP §2.2)| [PPAP-####]     | [Level # re-submit / waiver]| [date]      | [name]     | ☐        |
| Supplier control plan / incoming inspection      | [doc #]         | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Design History File (medical) / DMR              | [DHF / DMR ref] | [Rev → Rev]              | [date]         | [name]     | ☐        |
| Validation / re-validation (pharma; GAMP 5)      | [protocol #]    | [executed]               | [date]         | [name]     | ☐        |

Required-update completeness rule: A CAPA cannot move to Closed until every applicable row in this table is closed. Rows not applicable should be marked N/A with rationale.
```

**Step 10 — Customer and regulatory notification (8D D2 cross-check + closure).**

```
10. CUSTOMER AND REGULATORY NOTIFICATION

Customer notification:
  - Customer:                          [name, plant]
  - Notification clock per CSR:        [Ford QOS 24 hr / GM 1927-44 PRR / Stellantis 8D / Toyota CSL2 — quote applicable clause]
  - Notification sent:                 [yes — date, channel, recipient / no — rationale]
  - 8D delivered to customer:          [date / not required]
  - Stop-ship / controlled-shipping disposition: [CSL-1 / CSL-2 / CS-I / CS-II / CL2 / CL3 entry and exit conditions and clocks]
  - Customer-approved corrective action: [yes — date / pending — date / N/A]

Regulatory notification (if applicable):
  - Authority:                         [FDA MDR / FDA FAR (pharma) / NHTSA Part 573 / FAA Service Difficulty Report / OSHA / EPA / regulated agency]
  - Notification clock:                [statute and date]
  - Submitted:                         [yes — date / no — rationale]
  - Field action / recall declared:    [yes / pending review / no]

PPAP impact (per AIAG PPAP 4th ed. §2.2):
  - Customer notification required?    [yes — per §2.2.1.1 trigger / no]
  - Submission level required?         [Level 1 / 2 / 3 / 4 / 5 / waiver]
  - Submission date:                   [target]
```

**Step 11 — Approvals.**

```
11. APPROVALS

Per config.yml → approval_authority_matrix:
  Quality Engineer (RCA owner):    __________________  Date: __________
  Process / Manufacturing Engineering: __________________  Date: __________
  Production / Operations:         __________________  Date: __________
  Supplier Quality (if upstream):  __________________  Date: __________
  Customer-facing Quality (if downstream): __________________  Date: __________
  EHS (if safety-linked):          __________________  Date: __________
  IT / OT (if cyber-linked):       __________________  Date: __________
  Quality Manager / Director:      __________________  Date: __________
  Customer Approval (if CSR requires): __________________  Date: __________
  Regulatory Affairs (if FDA / NHTSA / FAA / OSHA / EPA reporting): __________________  Date: __________
```

**Step 12 — Closure (after effectiveness verification).**

```
12. CLOSURE

Closure prerequisites (ALL must be true before Close-Effective):
  ☐  Effectiveness threshold met across the full sample window
  ☐  All Section 9 controlled-document updates complete and verified
  ☐  Section 7 read-across CAPAs opened and tracked (closure of this CAPA does not depend on those being closed, but they must be open and assigned)
  ☐  Customer / regulatory notifications closed per Section 10
  ☐  Operator / inspector requalification complete per Section 9 LMS row
  ☐  Lessons-learned record published per Section 7
  ☐  CAPA-board reviewer sign-off

Verified by:           [name]                           Date: _____________
Evidence:              [sample window dates; sample size; result vs. threshold; measurement source]
Closure disposition:   [Closed-Effective / Closed-Not-Effective-Reopened with new CAPA # / Re-opened — original CAPA continues]
Recurrence-window flag: [12-month auto-flag set in QMS — re-open trigger if same failure mode recurs]
```

**Step 13 — Related records.**

```
13. RELATED RECORDS

NCRs:                       [list]
Prior CAPAs (12–24 mo):     [list with same part family / process / supplier / characteristic]
Supplier 8Ds (upstream):    [list]
Customer 8Ds (downstream):  [list]
Read-across CAPAs opened:   [list]
Warranty claim cluster:     [Warranty Claim Analyzer cluster ID, if applicable]
Safety incident link:       [Safety Incident & Near-Miss Report #, if applicable]
OT cyber event link:        [OT Cyber Incident Response event ID, if applicable]
Vision-system alarm link:   [Vision Inspection Summary event ID, if applicable]
Quality report reference:   [Quality Report Generator weekly / monthly entry that surfaces this CAPA]
```

## QMS-platform export format

From `config.yml → qms_platform`, structure the output for native upload:

- **MasterControl / MasterControl Quality Suite** — CAPA process record; problem statement maps to MasterControl IS/IS-NOT form; 5-Why structured object with verified/inferred flag; corrective-action steps map to action-item objects with owner and due-date routing; effectiveness-verification plan triggers Verification of Effectiveness (VOE) record; document-update table maps to linked-document objects; customer / regulatory notification fields populate from MasterControl reportable-event sub-form.
- **ETQ Reliance** — CAPA quality event type; 8D fields map to ETQ 8D form; D3 containment / D5 corrective action / D7 preventive action map to separate action records with linked-task workflow; document-update table maps to ETQ change-management linked records; effectiveness verification triggers ETQ VOE workflow.
- **Plex / Plex Quality** — CAPA linked to part number, line/cell, customer, and supplier; control-plan update auto-prompts in Plex; PFMEA linkage via Plex Control Plan / FMEA module; supplier-CAPA flow into supplier portal.
- **Greenlight Guru (medical devices)** — CAPA linked to Design History File / Device Master Record; FDA QMSR / 21 CFR 820.100 audit trail; risk-management file update flag; complaint-record linkage (21 CFR 820.198); MDR submission workflow if applicable.
- **Sparta TrackWise / TrackWise Digital** — Quality event type "CAPA"; structured fields for problem-solving framework; deviation-investigation linkage for pharma (21 CFR 211.192); validation-record linkage for GxP environments.
- **IQS / InfinityQS** — CAPA record linked to SPC chart, control plan, and PFMEA; Cpk evidence auto-pulled from ProFicient data collection; characteristic-level KPC/KCC flag.
- **Veeva Vault QMS (pharma / life sciences)** — CAPA lifecycle Draft → Review → Approved → Effective → Closed; 21 CFR Part 11 e-signature on approval line; deviation-investigation linkage; effectiveness-check workflow with Veeva QualityDocs linkage to SOPs.
- **AssurX** — CAPA process; document-control change-request linkage; supplier-management linkage; audit-finding linkage.
- **SAP QM module** — Quality notification (QN) type Q3 (internal) or F3 (customer complaint); inspection-lot linkage; quality-info-record update on supplier-driven CAPAs; document info record (DIR) update on controlled-document changes.
- **ServiceNow Manufacturing Commercial Operations — Quality Issues Management** — Quality Issue type "CAPA"; root-cause-analysis structured object; supplier-recovery linkage to ServiceNow Warranty Claims with AI Fraud Detection; field-action / TSB linkage to ServiceNow Field Service Management.
- **Platform-neutral output** — Standard markdown with YAML frontmatter keyed on: capa_no / status / severity / framework / customer_csr / trigger_type / kpc_kcc / occurrence_root_cause / detection_root_cause / containment_complete / ca_implemented / effectiveness_verified / docs_updated / read_across_count / target_close_date / actual_close_date.

## Output Requirements

- All 13 sections in order, with header completed (every field populated or explicitly TO BE COMPLETED)
- IS / IS-NOT problem statement with verbatim "as reported" preserved
- KPC ◆ / KCC ▲ designation explicit at the characteristic level with control-plan and PFMEA reference
- Customer CSR PPM-commitment exposure and controlled-shipping trigger evaluated and stated
- Containment table populated with quantities, dates, and evidence
- 5-Why with Verified / Inferred flag on every "why"; at least one parallel technique applied; occurrence and detection root causes separated
- PFMEA RPN before / after stated
- Corrective actions mapped to root-cause type per the match-to-root-cause rule
- Read-across search across all seven dimensions documented (result = found or clear)
- Effectiveness-verification plan with metric / threshold / sample plan / statistical basis / measurement-system validation
- Controlled-document update table with every applicable row closed before Close-Effective
- Customer / regulatory notification status closed per applicable CSR / regulation
- Approvals matrix populated per config.yml → approval_authority_matrix
- Closure prerequisites checklist explicit
- QMS-platform export format applied per config.yml → qms_platform
- If output saved to `outputs/`: file named `CAPA-[####]_[part-family]_[YYYY-MM-DD].md` (or platform-native format)

## Anti-Patterns to Avoid

- **Do not** stop the 5-Why at "operator error" or "supplier error." Both are symptoms; keep going until a system cause is reached.
- **Do not** combine containment, corrective, and preventive into one action list. They are different commitments with different evidence requirements and different closure clocks.
- **Do not** skip the detection root cause. The occurrence cause alone does not explain the escape; both need their own corrective actions.
- **Do not** mark a CAPA closed at "action implemented." Closure requires effectiveness verification with the defined sample, threshold, and window.
- **Do not** write "retrained the operator" as a corrective action without specifying what changed in the curriculum, who re-qualified, how re-qualification was evidenced, and the LMS record.
- **Do not** fabricate clause numbers, drawing revisions, PFMEA line items, or customer CSR clauses. Cite or mark TO BE COMPLETED for the owner to fill.
- **Do not** omit read-across. A failure mode in one place almost always exists in a related place; if read-across opens zero CAPAs, the search was probably not thorough.
- **Do not** close without updating the controlled-document set. Missing document updates is the most common audit finding against a "closed" CAPA.
- **Do not** rewrite the customer's or operator's verbatim complaint before classifying. The original wording is evidence; engineering paraphrase belongs in a separate field.
- **Do not** accept agentic-RCA structured output (Augie, Manufacturing Inspector Agent, Eigen, SAP Joule) as the final RCA without human cross-functional team validation and signed sign-off. AI assistance does not replace the team review.
- **Do not** close a CSR-driven CAPA without verifying the customer-approval row in Section 10. Customer-approved corrective action is a separate closure prerequisite from internal verification.
- **Do not** separate KPC/KCC designations from the characteristic. If a characteristic is KPC or KCC, the designation must travel with it through the entire CAPA record.
- **Do not** close a CAPA with active read-across CAPAs unassigned. Read-across CAPAs do not need to be closed for the parent to close, but they must be open and owned.
- **Do not** treat regulatory notification as the same workflow as customer notification. Regulatory clocks are statutory and cannot be deferred by the CAPA-board.

## Integration Notes

- **Pairs with Safety Incident & Near-Miss Report** — Safety-event contributing factors (HFACS organizational / supervisory / preconditions tier) flow into this template when the corrective action is system-level. The Safety Incident Report v2.0+ explicitly hands off system-cause corrective actions to this skill.
- **Pairs with Supplier Communication Drafter** — A supplier 8D response is the upstream input; the mirror CAPA on the receiving side covers incoming inspection, control-plan, and PPAP-impact assessment updates. The Supplier Communication Drafter `scar-8d-request` and `scar-response-review` templates are the matched companion records.
- **Pairs with Quality Report Generator** — Effectiveness-verification data lives in the weekly / monthly quality report; the QRG explicitly surfaces open CAPAs by status, age vs. target close, and effectiveness pass rate.
- **Pairs with Compliance Audit Prep** — CAPA records are the single most-examined evidence class in an ISO 9001 / IATF 16949 / AS9100 / FDA audit; Compliance Audit Prep pulls CAPA evidence for the audit-readiness package.
- **Pairs with Vision Inspection Summary** — Vision-system defect-class clusters with KPC/KCC linkage feed directly into the trigger field of this skill, including model-version traceability for re-train events.
- **Pairs with SOP Writer / Work Instruction Generator** — Controlled-document updates under Section 9 reference SOP and DWI revisions; SOP Writer Section 12 (requalification handoff) is the matching downstream artifact.
- **Pairs with Training Plan & Skill Matrix** — Operator requalification record under Section 9 is generated by Training Plan & Skill Matrix; the LMS ticket # closes the loop.
- **Pairs with Warranty Claim Analyzer** — Emerging-issue clusters from the warranty population are a common trigger; cluster ID populates the related-records section.
- **Pairs with OT Cyber Incident Response** — Cyber events with quality-record or product impact trigger a CAPA; OT-incident-response handoff feeds the trigger field.
- **Pairs with Tariff Impact Analysis** — Cost-of-poor-quality (COPQ) for the financial-exposure field can pull tariff-adjusted landed-cost figures from Tariff Impact Analysis if the affected part is in an exposed HTSUS classification.

## Success Metrics

- **On-time close rate at committed date:** target 90%+ (track CSR-specific close-clock compliance separately for Ford / GM / Stellantis / Toyota)
- **Effectiveness-verified close rate (first attempt):** % of CAPAs that pass the verification threshold on the first attempt — leading indicator of RCA quality
- **Recurrence rate within 12 months:** target < 5% on the same characteristic / part family / process
- **5-Why depth reaching system cause:** % of RCAs where the chain reaches a system cause (method / governance / design) rather than stopping at human or supplier error
- **Document-update completion at closure:** target 100% — missing doc updates is a default audit finding
- **Read-across rate:** % of CAPAs that open at least one read-across preventive action; near-zero indicates the read-across discipline is not being applied
- **Customer-approved corrective action turnaround:** average days from CA implementation to customer approval (for customer-driven CAPAs)
- **PFMEA RPN reduction:** average RPN-before vs. RPN-after on CAPA-driven PFMEA updates (leading indicator of effective error-proofing under IATF 16949 §10.2.4)
- **Audit finding rate against CAPAs:** target zero major NCs on CAPA records in registrar / customer / regulatory audits
- **Agentic-RCA validation rate:** % of agentic-RCA-assisted CAPAs where AI candidate causes were either accepted-after-validation or rejected-with-rationale (no "accepted without review" entries)

## Example Output

**Worked Pass-1 Quick CAPA Initiation** (example config: Summit Precision Components; three inputs supplied — trigger, problem description, immediate actions):

> *Input — Trigger:* "Ford program return, QPMT portal claim #FR-26-0418." *Problem:* "Customer reports weld porosity on bracket assy P/N 44-2207 found at their incoming dock; 3 of a 480-pc lot rejected; detected by customer incoming X-ray." *Immediate actions:* "Quarantined remaining 477 at our dock; verbally notified Ford SQE."

```
QUICK CAPA INITIATION — D0–D3

CAPA No:        CAPA-2026-0146  (provisional, ETQ Reliance numbering)
Title:          Weld porosity — bracket assy 44-2207 (Ford program)
Status:         Open — Containment in progress
Severity (prov):Critical   [from config: weld-porosity → KPC ◆, critical]
Customer CSR:   Ford Q1 — PPM commitment 15; portal Covisint/QPMT; notification clock 24h
Trigger:        Ford QPMT claim #FR-26-0418     Opened: 2026-06-29   D3-due: 2026-07-02

PROBLEM (IS / IS-NOT — first cut)
  As reported (verbatim): "weld porosity on bracket assy P/N 44-2207 found at incoming dock;
    3 of 480 rejected; customer incoming X-ray."
  IS:     weld porosity · WELD-1 robotic weld cell (critical) · lot of 480 ·
          3 confirmed reject / 477 in containment · detected at CUSTOMER incoming X-ray
  IS NOT: TO BE COMPLETED — other lots, other weld stations, other part numbers
          not yet verified clear

KPC/KCC FLAG: weld-porosity = KPC ◆ (critical). Escape against Ford Q1 PPM-15 commitment →
  controlled-shipping risk; PPM-commitment clock active.

CONTAINMENT (D3)
  | # | Action                              | Scope      | Owner        | Started    | Evidence       |
  | 1 | Quarantine remaining lot at dock    | 477 pc     | TO COMPLETE  | 2026-06-29 | hold tag #__   |
  | 2 | Sort-and-X-ray outbound + WIP       | TO COMPLETE| TO COMPLETE  | —          | sort record #  |
  | 3 | Notify Ford SQE (formal, portal)    | Ford, QPMT | Supplier QE  | 2026-06-29 | QPMT ref #__   |
  | 4 | Secure stock at Ford incoming       | TO COMPLETE| Ford SQA     | —          | customer hold  |

CLOCKS THAT JUST STARTED
  - Ford Q1 notification: 24h (formal portal entry due 2026-06-30)
  - Internal D3 containment certification: due 2026-07-02
  - Regulatory: none (no safety/recall implication identified yet — confirm in Pass 2)

PASS-2 GAP LIST
  1. Occurrence + detection root cause (D4) — why porosity produced AND why our EOL did not
     catch it (customer caught it = a detection escape, scored separately)
  2. Matched corrective/preventive actions (D5–D7); weld-parameter + shielding-gas review
  3. Effectiveness-verification plan (X-ray reject rate / consecutive lots / window)
  4. Controlled-document set: WELD-1 control plan, PFMEA (RPN), weld WI, vision/X-ray threshold
  5. Related history: prior weld-porosity NCRs/CAPAs on this family (12–24 mo)
```
>
> Escalation check: KPC ◆ + Critical severity + customer-CSR PPM escape + customer-owed 8D → **four of five triggers fire; proceed to Full CAPA (Pass 2) immediately.** The record is open, containment is staged, and the Ford 24h clock is captured — within minutes of the claim landing, before root cause is known.

For a complete Pass-2 record, run the 13-section process above; Pass 1 fields carry forward unchanged.
