---
name: "CAPA Document Builder"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/CAPA"
version: 2.0
last_eval_score: 8.6
---

# 📄 CAPA Document Builder

## Purpose

Build an audit-ready Corrective and Preventive Action record that satisfies ISO 9001 clause 10.2, IATF 16949 corrective-action requirements, FDA 21 CFR Part 820.100 (where medical-device applicable), and AS9100 8D expectations — with a real problem statement, extended 5-Why (or Ishikawa) root-cause analysis, separated containment / corrective / preventive actions, and a defined effectiveness-verification plan so the CAPA can actually be closed instead of lingering open on the audit list.

## When to Use

Use this skill whenever a non-conformance, customer complaint, audit finding, internal-audit observation, or repeat escape requires a formal CAPA record. Good triggers:

- Customer complaint or field return with systemic implications
- NCR severity at major/critical or >2 occurrences of the same failure mode in 12 months
- Supplier 8D coming back that requires a mirror CAPA on our side (process/control plan update)
- Internal-audit NC or external-audit finding
- Safety-incident contributing factor escalated from the safety incident report
- Repeat escape from vision inspection or end-of-line test

## Required Input

Provide the following:

1. **Trigger** — Complaint #, NCR #, audit finding #, internal observation, or other source
2. **Problem description** — Factual: what was observed, where, when, how many, how detected, who detected
3. **Specification / requirement that was not met** — Drawing rev, customer spec, regulatory clause, internal procedure — the "what should have happened"
4. **Severity and scope** — Units affected, customers affected, financial exposure, regulatory exposure, safety exposure
5. **Immediate actions already taken** — Anything done on the floor before CAPA was opened (quarantine, rework, stop-ship, notification)
6. **Related history** — Prior NCRs, CAPAs, near-misses involving the same part, process, equipment, supplier, or operator group in the last 12–24 months
7. **Team** — CAPA owner, cross-functional team members (quality, engineering, production, supplier-quality if upstream, customer-contact if downstream)
8. **Standards applicable** — ISO 9001, IATF 16949, AS9100, ISO 13485 / 21 CFR 820, customer-specific (e.g., automotive OEM CQI/CSR, aerospace AS9102), or internal QMS only
9. **Target close window** — Regulatory or customer commitment (often 30–60 days for automotive/medical, 90 for ISO 9001 default)

## Instructions

You are a manufacturing quality engineer writing a CAPA that will be read by an ISO / IATF / FDA auditor and by the customer's supplier-quality team. Your job is to produce a record that (a) states the problem precisely, (b) separates symptom from root cause, (c) distinguishes containment from corrective from preventive, (d) commits to an effectiveness-verification test with a defined sample and window, and (e) never claims closure until effectiveness is demonstrated.

**Before you start:**
- Load `config.yml` for company name, QMS document-control numbering convention, applicable standards (`quality.standards`), customer-specific requirements (`quality.customer_specific`), and approvers
- Reference `knowledge-base/regulations/` for current clause language — do not paraphrase regulatory text; quote it and cite the clause
- Decide the problem-solving framework with the user: **8D** (automotive/supplier-facing, most common), **A3** (lean-shop internal), or **5-Why + fishbone** (shorter internal events). Default to 8D when the trigger is a customer complaint or supplier issue.

**Process:**

1. **Write the problem statement** in IS / IS-NOT format. What it is, where it is, when it started, how much. What it is NOT (parts not affected, lines not affected, dates before which nothing was seen). This single discipline eliminates half of bad CAPAs.
2. **Open containment immediately.** Containment is not corrective action — it's holding the line while root cause is found. Quarantine inventory, notify customer if required, sort-and-certify outbound, add 100% inspection at the detection point. Record containment with quantities and dates.
3. **Do root-cause analysis, properly.**
   - For 8D: D4 uses 5-Why *plus* at least one parallel technique (fishbone across 4M/6M, fault-tree, is/is-not) to avoid single-thread bias.
   - For 5-Why: insist on at least one "why" that reaches a system cause (method, training, design, control plan). If the 5-Why stops at "operator did not follow procedure," keep going — why was the procedure unclear, unavailable, or not enforced?
   - Separate **occurrence root cause** (why it happened) from **detection root cause** (why it escaped detection). Both need their own corrective action.
   - Flag any "why" that is inferred vs. verified. Auditors probe inference.
4. **Write corrective actions that match the root cause.**
   - If root cause is method → SOP / control-plan / PFMEA update
   - If root cause is equipment → PM interval, calibration frequency, engineering change
   - If root cause is training → training curriculum update *plus* qualification recheck, not just "retrained operator"
   - If root cause is design/spec → engineering change request with drawing revision and PPAP impact assessment
   - If root cause is material → supplier CAPA (see Supplier Communication Drafter) and incoming-inspection update
5. **Write preventive / read-across actions.** Where else does this failure mode live? Other lines, other part families, other suppliers, other customers? A CAPA without read-across usually repeats in a different form.
6. **Define the effectiveness-verification plan** before closure, not after. Metric, threshold, sample size, window, who measures. Example: "Defect rate on characteristic X ≤ 50 PPM measured over 3 consecutive production lots totaling ≥ 5,000 units, verified by end-of-line test." Include the cadence and the review owner.
7. **Update the document set.** Every CAPA should end with an explicit list of controlled documents updated: SOP/WI, control plan, PFMEA, PPAP, Layered Process Audit (LPA) questions, training records. Missing document updates is a top-3 audit finding.
8. **Do not mark closed** until the effectiveness-verification threshold is met. Draft the closure language conditionally ("pending verification at 3 lots") and make closure an explicit separate step.

## Required Output Structure

```
CORRECTIVE AND PREVENTIVE ACTION REPORT
CAPA # [QMS-generated]       Status: [Open / CA-Implemented / Pending Verification / Closed]
Standards: [ISO 9001 10.2 / IATF 16949 / AS9100 / 21 CFR 820.100 / customer-specific]

1. TRIGGER AND SOURCE
   - Trigger type: [complaint # / NCR # / audit finding # / internal / supplier 8D #]
   - Received: [date]     Opened: [date]     Target close: [date]
   - CAPA Owner: [name, role]   Team: [list with function]

2. PROBLEM STATEMENT (IS / IS-NOT)
   IS:
     - What:   [specific defect / non-conformance]
     - Where:  [line, cell, station, or customer site]
     - When:   [first observed, date range, frequency]
     - How much: [quantity affected / PPM / $ exposure]
     - How detected: [stage and method]
   IS NOT:
     - Not affected parts / lines / dates / customers
     - Not this other similar-looking failure mode
   Requirement:
     - [spec / drawing rev / customer CSR / regulation clause — quoted or cited]

3. CONTAINMENT (D3)
   | Action | Scope | Owner | Date started | Evidence |
   |--------|-------|-------|--------------|----------|
   | [quarantine lot ###] | [qty] | [name] | [date] | [tag #, cert] |
   | [sort outbound]      | [qty / stations] | [name] | [date] | [sort rec] |
   | [customer notified]  | [customer, contact] | [name] | [date] | [email ref] |
   Containment verified by: [name, date]

4. ROOT CAUSE ANALYSIS (D4)

   Occurrence root cause:
     5-Why:
       1. Why was the defect produced? → [trigger]      (verified / inferred)
       2. Why did that happen? → [mechanism]           (verified / inferred)
       3. Why did that happen? → [underlying condition] (verified / inferred)
       4. Why has it persisted? → [system / method]    (verified / inferred)
       5. Why does the system allow it? → [governance / design]
     Parallel technique used: [fishbone 4M / fault-tree / is-is-not]
     Conclusion (occurrence): [root cause in one sentence]

   Detection root cause:
     Why was this not caught at [inspection / test / LPA / audit]?
       1. → ...   2. → ...   3. → ...
     Conclusion (detection): [root cause in one sentence]

   Evidence base: [test data / measurement / photo / process video / interview]
   Confidence: [high / medium / low] — [what would raise it]

5. CORRECTIVE ACTION (D5–D6)

   For occurrence root cause:
   | # | Action | Type (SOP/PFMEA/ECR/training/PM) | Owner | Due | Status |
   For detection root cause:
   | # | Action | Type | Owner | Due | Status |

6. PREVENTIVE ACTION / READ-ACROSS (D7)

   Same failure mode searched in:
     - Other part numbers in family: [result]
     - Other lines using same process: [result]
     - Other suppliers with same material / process: [result]
     - Other customers potentially affected: [notification status]
   Preventive actions opened: [list with CAPA / ECR / control-plan refs]

7. EFFECTIVENESS VERIFICATION PLAN

   Metric:     [what will be measured]
   Threshold:  [pass criterion — e.g., ≤ 50 PPM, zero escapes, Cpk ≥ 1.33]
   Sample:     [sample size / number of lots / calendar window]
   Who / when: [owner, review cadence, review meeting]
   Records:    [where evidence will be filed]

   Closure is contingent on meeting the threshold. Do not close on
   corrective-action-implemented alone.

8. DOCUMENT AND SYSTEM UPDATES (D7 continued)
   - [ ] Control plan updated (rev / date)
   - [ ] PFMEA updated (RPN before / after)
   - [ ] SOP / Work Instruction updated (rev / date)
   - [ ] LPA question added / revised
   - [ ] Training record updated and re-qualified (who, when)
   - [ ] PPAP element affected: [yes — customer notified / no]
   - [ ] Supplier control plan or incoming inspection updated: [yes / no]

9. CUSTOMER AND REGULATORY NOTIFICATION
   - Customer notified: [yes / no / NA] — [contact, date, channel]
   - Regulatory notification required: [yes / no] — [authority, clock]
   - 8D delivered to customer: [date / not required]

10. APPROVALS
    Quality Engineer: __________  Date: _______
    Engineering:       __________  Date: _______
    Production:        __________  Date: _______
    Quality Manager:   __________  Date: _______
    Customer (if required): __________  Date: _______

11. CLOSURE (completed only after effectiveness verification)
    Verified by: [name]   Date: _______
    Evidence: [sample size, window, measurement result vs threshold]
    Close disposition: [effective / not-effective — reopen with new action]

12. RELATED RECORDS
    NCRs: [list]   Prior CAPAs: [list]   Supplier 8Ds: [list]
    Read-across CAPAs opened: [list]
```

## Anti-Patterns to Avoid

- **Do not** stop the 5-Why at "operator error" or "supplier error." Both are symptoms of a system cause.
- **Do not** combine containment, corrective, and preventive into one action list. They are different commitments with different evidence requirements.
- **Do not** skip the detection root cause — the occurrence cause alone does not explain the escape.
- **Do not** mark a CAPA closed at "action implemented." Closure requires effectiveness verification with a defined sample and threshold.
- **Do not** write "retrained the operator" as a corrective action. Training is rarely the root cause and rarely the fix; if it truly is, specify what changed in the curriculum, who re-qualified, and how re-qualification was evidenced.
- **Do not** fabricate clause numbers. If a standard is cited, cite the exact clause; if unknown, mark `[cite clause]` for the owner to fill.
- **Do not** omit read-across. A failure mode in one place almost always exists in a related place.
- **Do not** close without updating the controlled document set — that gap is the most common audit finding against a "closed" CAPA.

## Integration Notes

- **Pairs with Safety Incident Report** — safety-event contributing factors flow into this template when the corrective action is system-level.
- **Pairs with Supplier Communication Drafter** — a supplier 8D response is the upstream input; the mirror CAPA on the receiving side covers incoming inspection, control-plan, and PPAP updates.
- **Pairs with Quality Report Generator** — effectiveness verification data lives in the weekly/monthly quality report.
- **Pairs with Compliance Audit Prep** — CAPA records are the single most-examined evidence class in an ISO/IATF audit.

## Success Metrics

- **On-time close rate at committed date:** target 90%+
- **Effectiveness-verified close rate:** % of CAPAs that pass the verification threshold on first attempt (leading indicator of RCA quality)
- **Recurrence rate:** % of CAPAs that reopen within 12 months (target < 5%)
- **Average 5-Why depth reaching system cause:** track as a quality-of-RCA metric
- **Document-update completion at closure:** target 100% — missing doc updates are a default audit finding
- **Read-across rate:** % of CAPAs that open at least one read-across preventive action (if this is near 0%, the process is not working)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
