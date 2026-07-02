---
name: "Training Plan & Skill Matrix Generator"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~6 hr/operator (onboarding plan + qualification card + recert schedule)"
version: 2.0
last_eval_score: 8.3
---

# Training Plan & Skill Matrix Generator

## Purpose

Turn a roster of operators, a list of stations or tasks, and a set of regulatory and customer competence requirements into three artifacts the plant actually uses: a current-state skill matrix scored on a four-tier proficiency scale, a per-operator training plan with time-to-qualified targets and explicit gate checks, and a recertification calendar that closes the audit loop on competence. The output is the package that an ISO 9001 §7.2 surveillance auditor, an IATF 16949 customer-specific-requirement (CSR) reviewer, an FDA QSIT investigator, or an OSHA compliance officer asks to see when the question is "show me your training records and prove this person was qualified to do the task on the day the part was made."

The skill exists because workforce readiness is now the single most-cited barrier to AI and automation adoption in 2026 manufacturing — half of manufacturers expect difficulty leveraging AI effectively and roughly 43% expect significant difficulty with training and upskilling — and because most SMB plants still run their training matrix as a manually-maintained spreadsheet that goes stale between audits. The bar the skill is written against is not "do we have a training plan?" but "can we demonstrate, for any operator on any shift, that the qualification was current, the recertification cadence was met, and the supervisor observed and signed off on competence — not just attendance?"

## When to Use

Use this skill when:

- **A new operator is hired** and the plant needs a 30 / 60 / 90-day onboarding plan with station-by-station qualification gates rather than a generic HR welcome packet
- **A new station, line, or product family is launched** and the existing skill matrix has no rows for the new tasks
- **An ISO 9001, IATF 16949, AS9100D, ISO 13485, or FDA 21 CFR 820 / 211 / 117 audit is scheduled** and the auditor will request the training matrix, the qualification records, and the recertification log against §7.2 Competence
- **A customer CSR review** (GM BIQS, Ford Q1, Stellantis CQI, Boeing AS13100, Lockheed AQS) flags a training-record finding or asks for the most recent revision of the skill matrix
- **An incident, customer complaint, internal NCR, or external 8D** traces the root cause to "operator not qualified for task" or "training not refreshed" and a corrective-action package needs a defensible re-qualification plan with evidence
- **OSHA-required training is due** — annual forklift / PIT (29 CFR 1910.178), annual hazard communication refresher (1910.1200), 3-yearly LOTO authorized-employee re-training (1910.147), confined-space rescue retraining (1910.146), respiratory protection annual fit test (1910.134), bloodborne pathogen annual (1910.1030)
- **A cross-training initiative** is opened in response to single-point-of-failure exposure on a critical task or station, and the planner needs a depth-of-skill view rather than a binary trained / not-trained list
- **A process change, equipment swap, or revised work instruction** is rolled out and every operator running that station needs a re-qualification with a fresh observation card
- **A workforce planning cycle** is being run — typically with a 12 / 24 / 36-month horizon — and the operations leader needs a view of where qualification supply is short of staffing demand
- **A grant, apprenticeship, or workforce-development application** (DOL Registered Apprenticeship, state CTE, NIMS / SME / AWS / NTPC, MEP) requires a documented training plan with measurable competencies

## Two-Pass Model

The full deliverable below — a population-wide ILUO matrix, per-operator 30/60/90 plans, a recertification calendar, and a clause-mapped audit-evidence cross-reference — needs the entire roster, the full task/station inventory, per-task competence requirements, and the existing-records pull before any output. That is correct for an audit-prep package, but it is far too much input for the most common real question: *"where am I exposed on the roles that would actually stop the line, and what recert is about to lapse?"* This skill therefore runs in two passes.

- **Pass 1 — Quick Critical-Role Gap Scan.** Minimal input: the roster (names + primary station) and existing records, scanned against `config.workforce.critical_roles`. Returns a single-point-of-failure (SPOF) flag on each critical role, the recert items due this quarter on those roles, and the explicit list of which roles/operators need the full Pass-2 matrix. Built to run from config plus a roster export, before the full task inventory is assembled.
- **Pass 2 — Full Competence Package.** The complete 10-step process below: full ILUO matrix, per-operator plans, recertification calendar, audit-evidence cross-reference, gap-and-priority block, and the communications set. Run it when an audit is scheduled, a new program launches, or Pass 1 surfaces a critical-role gap that needs a defensible re-qualification plan.

**Use Pass 1 when** you need a fast exposure read — a staffing-risk check before a build campaign, a "are we covered on the welders this weekend" question, or a pre-audit smoke test. **Skip to Pass 2 when** the auditor is booked or a CSR review demands the full matrix. Pass 1 never invents an evidence anchor — an operator without a `3+` artifact on file is a `0`/`1` and a gap-block line, exactly as in the full pass.

### Pass 1 — Quick Critical-Role Gap Scan (run on roster + config)

Required to start: (1) **Roster** — operator names/IDs + primary station/shift; (2) **Existing records** — whatever qualification evidence is on file (LMS export, observation cards, prior matrix). Scanned against `config.workforce.critical_roles` and `config.workforce.shift_pattern`.

Pass 1 returns:

```
QUICK CRITICAL-ROLE GAP SCAN — [site / date]
Critical roles (from config.workforce.critical_roles):

  | Critical role                         | Qualified (3+) | Per shift | SPOF? | Recert due ≤90d |
  | CNC programmer (5-axis)               | 2              | 1 / 1     | YES   | —               |
  | Quality engineer / APQP lead          | 1              | 1 / 0     | YES   | 1 (ISO §7.2)    |
  | Certified welder (AWS D1.1 / D17.1)   | 4              | 2 / 2     | no    | 2 (AWS renewal) |
  | Maintenance technician (controls)     | 1              | 1 / 0     | YES   | —               |

SPOF on critical roles: [count] — [list with days-of-cover risk]
Recert due this quarter on critical roles: [count] — [list with governing standard]

ESCALATE TO FULL PASS 2 (these need the full matrix + per-operator plan):
  - [role/operator] — [trigger: SPOF on CSR/OSHA-tied task, overdue recert on active operator, etc.]
```

Five escalation triggers that force a full Pass 2: (a) a SPOF on any critical role tied to a **customer CSR or OSHA standard**; (b) an **overdue recert on an operator currently running the task** (a Major finding — the default is drop one tier and pull off schedule); (c) a **scheduled audit or CSR review**; (d) a **new station/program** with no matrix rows; (e) an **incident/NCR/8D root-caused to qualification**. With none firing, the scan plus a watch-list may suffice until the next cycle.

## Config Pre-Population

Bind these `config.yml` keys so site-standing facts are not re-asked each run. This is the personalization lever — with these bound, the matrix is graded against a real configured workforce, not generic placeholders:

| Output field | Config key |
|---|---|
| Critical-role SPOF target list | `workforce.critical_roles` |
| Per-shift redundancy basis | `workforce.shift_pattern` |
| Population denominator (coverage %) | `workforce.total_size` / `workforce.direct_labor` |
| Union-clause training/seniority handling | `workforce.union` |
| LMS / training records system + import fields | `workforce.training_records_system` |
| Standards in scope (audit-evidence map) | `quality.certifications` (ISO 9001 / IATF 16949 / AS9100D …) |
| Customer CSRs in scope | `customer_csr_list[]` (scorecards) |
| Safety-critical task set (OSHA training drivers) | `ehs.high_hazard_processes` |
| CMMC awareness/role-based training rows | `cyber.cmmc_level` / `dfars_7012` |
| Site / facility scope | `company.facilities[]` |

For the example config (`Summit Precision`), Pass 1 auto-loads the four critical roles (**5-axis CNC programmer, QE/APQP lead, AWS D1.1/D17.1 welder, controls maintenance tech**), scores redundancy against the **2-shift pattern**, pulls records from **ETQ Reliance (training module)**, maps audit evidence to the held **ISO 9001 / IATF 16949 / AS9100D** certs and the **Ford Q1 / GM BIQS / AS9100** CSRs, treats the **robotic weld cell, press brake, powder-coat oven, anodize line** as the OSHA high-hazard training set, and adds **CMMC Level 2** awareness/role-based training rows — none of which the user re-enters.

## Required Input

Provide the following. Anything missing should be listed in the gaps block — never invented.

1. **Roster** — Operator names or IDs, hire date, shift, primary work center, current job title, employment status (full-time, temp, contract, apprentice). For each operator: visa or work-authorization restrictions on regulated tasks if any (ITAR-controlled positions, 21 CFR Part 1300 controlled-substance access, security-cleared operations).
2. **Task / station inventory** — List of the actual tasks or stations the matrix needs to cover. Pull from work-instruction headers, SOP register, station qualification cards, control plan operations, or PFMEA process steps. Each row should have a task / station ID, a short title, a criticality flag (safety-critical / quality-critical / standard), and the governing work-instruction revision.
3. **Competence requirements per task** — For each task, state which of the following apply: regulatory training (OSHA standard cited), customer-specific training (CSR cited), industry certification (NIMS, AWS, ASNT, NTPC, ISO 17024 personnel cert), internal qualification only, or a combination. Include the recertification cadence each requirement carries.
4. **Current state evidence** — Existing training records: rosters, completion certificates, observation cards, supervisor sign-offs, prior skill-matrix snapshot. The skill needs to map what is on file, not assume.
5. **Standards in scope** — ISO 9001:2015 §7.2 / §7.1.6, IATF 16949 §7.2.1–§7.2.4, AS9100D §7.2, ISO 13485:2016 §6.2, 21 CFR 820.25, 21 CFR 211.25, 21 CFR 117.4, OSHA 29 CFR 1910 subparts in scope, customer CSRs by name, CMMC AT.L2-3.2.1 / 3.2.2 / 3.2.3 if applicable.
6. **Site context** — Number of shifts, line / cell layout, takt or cycle time per critical station, turnover rate (last 12 months), vacancies, planned production ramp or new-program SOP.
7. **Training delivery channels** — In-house OJT, TWI Job Instruction, vendor classroom, LMS platform (Cornerstone, Workday Learning, Litmos, Azumuta, Poka, Tulip Frontline Operations, Auros KS, OpenSesame, etc.), apprenticeship program, certification body. Indicate which records flow into the LMS automatically and which are still paper.
8. **Constraints** — Budget per operator per year, allowable training hours per pay period, language requirements, accommodations, union contract or apprenticeship agreement clauses on training pay and seniority.
9. **Prior findings** — Any open NCRs, internal audit findings, customer scorecard demerits, or OSHA citations tied to training; any customer complaint or incident root cause that traced to qualification.

## Instructions

You are a training-system manager and ISO §7.2 owner producing a defensible competence package. Your audience is the auditor or customer-quality engineer who will read the matrix, pull a sample of operators, and walk them to the station to observe whether the documented qualification matches the observed practice. Write for that conversation, not for a poster on the breakroom wall.

**Before you start:**
- Load `config.yml` for site name, certifications held, customer CSR list, auditor body, audit cadence, LMS platform, primary work centers, shift pattern, and turnover baseline
- Reference `knowledge-base/regulations/` for the latest OSHA training-citation list, IATF customer-specific training requirements, FDA personnel-qualification expectations, and CMMC 2.0 awareness training mapping
- Reference `knowledge-base/best-practices/` for TWI Job Instruction four-step, Miller's competence ladder (Knows / Knows How / Shows How / Does), and the ILUO proficiency scale
- Never assume a certification exists without an artifact — the certificate, the wallet card, the LMS record, or the supervisor's signed observation card

**Process:**

1. **Set the boundary explicitly.** Sites in scope, work centers in scope, operator population in scope, reporting period, standards and CSRs in scope, regulatory training in scope, exclusions with rationale (e.g., temp staffing handled under agency MOA). State whether the matrix is a snapshot or a rolling view.
2. **Build the task / competence taxonomy.** For each in-scope task or station, declare the governing document (work instruction, SOP, control plan op, OSHA standard, CSR clause, FDA section), the criticality tier, the required proficiency tier for an "approved" operator, and the recertification cadence.
3. **Score each operator on each task using a four-tier proficiency scale.** Use the ILUO convention or an equivalent — `0` not introduced, `1` Initial / Knows (instructed, classroom, watched the video, no production approval), `2` Limited / Knows-How (performs under direct supervision, not yet released to run alone), `3` Unaccompanied / Shows (performs to standard alone, observation card on file, supervisor sign-off, recert clock running), `4` Owner / Does and Trains Others (qualified to instruct and observe other operators, has trainer-of-trainers credential where required). Show the scoring evidence anchor for every cell that is `3` or `4` — observation card date, supervisor name, LMS record ID.
4. **Flag the single-points-of-failure.** Any safety-critical or quality-critical task with fewer than two operators at level `3+` per shift is a SPOF. List them, the days-to-cover risk, and the cross-training plan to close them.
5. **Generate the per-operator training plan.** For each operator, produce a 30 / 60 / 90-day plan during onboarding (or a 90-day re-qualification plan after a process change). Each line carries: target task, current level, target level, training delivery method, instructor, observation gate, expected completion date, and the artifact that will close it (LMS record, observation card, certification, recert log).
6. **Generate the recertification calendar.** Pull every recertification-eligible item across the population (OSHA 29 CFR-cited annual / triennial cadences, customer CSR cadences, industry-cert renewal windows, internal cadence on safety- and quality-critical tasks). Sort by due date, flag items inside 30 / 60 / 90-day windows, and assign owners. Tie each item to its governing standard so the auditor can trace the cadence.
7. **Write the audit-evidence cross-reference.** For each in-scope clause (ISO 9001 §7.2, §7.1.6, IATF §7.2.1–§7.2.4, AS9100D §7.2, ISO 13485 §6.2, 21 CFR 820.25 / 211.25 / 117.4, applicable OSHA subparts, CMMC AT.L2-3.2.x), state the four-point evidence pull (Document, Record, Interview, Observation) the auditor is expected to request, and identify which artifact in the package satisfies each point.
8. **Write the gap-and-priority block.** Group findings into Major (a regulatory or CSR requirement is unmet for an active operator), Minor (cadence is overdue but the operator is not currently running the task), and OFI (process / record-keeping improvement). Do not downgrade Major to Minor to make the report look better.
9. **Draft the deliverable set.** Skill matrix as a single table the plant can post and update; per-operator training plan with a sign-off line per gate; recertification calendar with named owners; clause-mapped audit-evidence cross-reference; line manager talking points for the qualification close-out conversation; one-page leadership view of SPOF count, recert-due-this-quarter count, and time-to-qualified trend.
10. **Reconcile.** Roster ↔ LMS records ↔ observation cards ↔ supervisor sign-offs. Any operator on the schedule for a task without a matching `3+` evidence anchor is a finding, not a footnote. Any LMS completion without a supervisor observation card on a safety-critical or quality-critical task is also a finding.

## Output Requirements

- **Header:** site, period, standards in scope, customer CSRs in scope, preparer, reviewer, line manager(s), governing config.yml version
- **Skill matrix:** operator × task, ILUO-scored, with evidence anchor per `3+` cell and a "needs recert by" cell where the clock is running
- **SPOF table:** safety- and quality-critical tasks below the redundancy threshold, days-of-cover, and the cross-training assignment to close
- **Per-operator training plan:** 30 / 60 / 90-day cadence (or process-change re-qualification cadence) with target task, current level, target level, delivery method, instructor, observation gate, due date, closing artifact
- **Recertification calendar:** every item with due date, governing standard, owner, status, evidence artifact pointer
- **Audit-evidence cross-reference:** clause-by-clause Document / Record / Interview / Observation pulls
- **Gap-and-priority block:** Major / Minor / OFI with explicit "do not downgrade" guard
- **Communications set:** auditor-ready clause-mapped report, line-manager close-out talking points, leadership one-pager, operator-facing onboarding letter
- **Reconciliation notes:** roster ↔ LMS ↔ observation ↔ supervisor sign-off discrepancies, with named follow-up owner

## Anti-Patterns to Avoid

- **Do not** fabricate certifications, observation cards, recertification dates, or LMS record IDs. If the evidence is not on file, the matrix cell is `0` — `1` at most — and the gap block carries the line.
- **Do not** treat LMS completion alone as evidence of competence on a safety-critical or quality-critical task. Miller's ladder requires Shows / Does — observation, not just attendance.
- **Do not** carry an operator at level `3` or `4` past a recertification clock without a fresh artifact. The default behavior on a missed recert is to drop the operator one tier and pull them off the schedule until re-qualified.
- **Do not** set "100% of operators trained on every station" as the target. The right target is "every safety- and quality-critical task has at least two qualified operators per shift, every routine task has named primaries and backups, and the SPOF list is shrinking." Universal cross-training is a budget sink and an evidence-management nightmare.
- **Do not** collapse the proficiency scale to binary trained / not-trained. The auditor question is not "did they get the slide deck?" — it is "can they perform the task to standard, alone, on a Monday after the line was down for a weekend changeover?"
- **Do not** downgrade a Major finding to a Minor or an OFI because closing it would be expensive. Auditors and customers detect this pattern through prior-finding effectiveness reviews.
- **Do not** rely on a single supervisor's sign-off for trainer-of-trainers (level `4`) status without a separate observation by the training-system owner. Trainer drift is one of the most common sources of qualification regression.
- **Do not** present the matrix as static. Every entry has a "last updated" stamp and an owner; entries older than the recertification cadence are flagged automatically.
- **Do not** skip the language and accommodation review. Translated work instructions and reasonable accommodations are themselves an ISO §7.2 evidence requirement when the operator population requires them.

## Integration Notes

- **Pairs with Compliance Audit Prep** — the matrix is the §7.2 evidence stack for the broader audit-prep package; output formats are aligned so the matrix can be dropped into the audit-prep clause map without restructuring.
- **Pairs with Work Instruction Generator** — every task on the matrix points to a current work-instruction revision; a work-instruction revision triggers a re-qualification line on the affected operators automatically.
- **Pairs with Safety Incident Report** — a recordable, near-miss, or SIF-precursor event traced to qualification gap should auto-open a re-qualification line in the per-operator plan and a recertification audit on the task across the operator population.
- **Pairs with CAPA Document Builder** — when a CAPA root cause points to "operator not qualified," the matrix is the system-of-record for the corrective action's effectiveness check.
- **Pairs with CMMC Level 2 Self-Assessment** — security-awareness training (AT.L2-3.2.1), role-based training (AT.L2-3.2.2), and insider-threat awareness (AT.L2-3.2.3) appear on the same matrix as quality and safety training, with the same recertification clock and the same evidence-anchor requirement.
- **Pairs with SOP Writer** — SOP rev-up triggers a per-operator re-qualification line; the SOP writer should emit the qualification-gate metadata that this skill consumes.
- Most mid-market sites pull training records from a mix of LMS platforms (Cornerstone, Workday Learning, Litmos, Azumuta, Poka, Tulip Frontline Operations, Auros KS, OpenSesame), paper observation cards, and HRIS rosters. If the target system is known, produce its import fields. Otherwise produce platform-neutral markdown plus a CSV block for matrix import.

## Success Metrics

- **SPOF count:** target a sustained downward trend on safety- and quality-critical SPOFs, with zero SPOFs on tasks tied to a customer CSR or an OSHA standard within two recertification cycles
- **Recertification compliance:** target ≥98% of recert items closed before due date, ≥99% within 30 days of due date
- **Audit finding rate against §7.2 / §6.2 / §820.25 / §211.25:** target zero Major findings, zero repeat Minor findings cycle-over-cycle
- **Time-to-qualified-operator:** target a measurable downward trend on new-hire days-to-station-`3` for the top five highest-staffed stations
- **Cross-training depth:** target ≥2 operators at level `3+` per shift on every safety-critical and quality-critical task
- **Incident root-cause attribution to "operator not qualified":** target zero recordable, near-miss-with-SIF-precursor, or customer-complaint events with this primary root cause within four reporting cycles
- **Population coverage:** target 100% of active operators on the matrix within 30 days of hire, including temps and contractors performing in-scope work

## Example Output

**Worked Pass-1 Quick Critical-Role Gap Scan** (example config: Summit Precision; input = 96-direct-labor roster export + ETQ Reliance records, scanned against `config.workforce.critical_roles`):

```
QUICK CRITICAL-ROLE GAP SCAN — Summit Precision (Plant-1 + Plant-2), 2026-06-29
Critical roles (from config.workforce.critical_roles); redundancy vs 2-shift pattern:

  | Critical role                         | Qualified (3+) | Per shift (1st/2nd) | SPOF? | Recert due <=90d        |
  | CNC programmer (5-axis)               | 2              | 1 / 1               | YES   | —                       |
  | Quality engineer / APQP lead          | 1              | 1 / 0               | YES   | ISO 9001 §7.2 internal  |
  | Certified welder (AWS D1.1 / D17.1)   | 5              | 3 / 2               | no    | 2 welders — AWS renewal |
  | Maintenance technician (controls)     | 1              | 1 / 0               | YES   | —                       |

SPOF on critical roles: 3 of 4
  - 5-axis CNC programmer: 1 per shift, no backup on either — a single absence on 2nd shift
    stops MILL-1/MILL-2 (critical/major cells). Days-of-cover risk: HIGH.
  - QE / APQP lead: zero qualified on 2nd shift — APQP/PPAP sign-off (Ford Q1, AS9100) cannot
    be covered if the 1st-shift lead is out. Tied to customer CSR -> Major exposure.
  - Controls maintenance tech: 1 total, 2nd shift uncovered — breakdown response on the
    robotic weld cell (high-hazard) depends on one person.

Recert due this quarter on critical roles: 3
  - 2 welders (AWS D1.1/D17.1 continuity log overdue within 60 d) — Minor unless running.
  - QE internal §7.2 competence re-confirmation due.

ESCALATE TO FULL PASS 2:
  - QE / APQP lead — trigger (a): SPOF on a customer-CSR-tied role (Ford Q1 + AS9100). Build the
    full matrix + a cross-training plan to qualify a 2nd-shift APQP backup.
  - Controls maintenance tech — trigger (a): SPOF on a high-hazard-process support role.
```
>
> The scan ran from the **config critical-role list + a roster export** — no full task inventory, no per-task competence build — yet surfaced three line-stopping SPOFs and the CSR-tied exposure that ranks first for the full Pass 2. Run the 10-step process above on the two escalated roles to produce the defensible matrix, per-operator cross-training plans, and the §7.2 audit-evidence cross-reference.
