---
name: "Safety Incident & Near-Miss Report"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~40 min/incident"
version: 2.0
last_eval_score: 8.9
---

# Safety Incident & Near-Miss Report

## Purpose

Turn a raw account of a plant-floor safety event — whether an actual injury, a property-damage incident, a near-miss, or a SIF precursor — into a complete, audit-ready incident report. The report classifies OSHA recordable status with BLS SOII coding, triggers the right regulatory notification clocks (OSHA 8-hour / 24-hour / 7-day; state workers'-comp first-report-of-injury), cross-references the active JSA/JHA and PPE hazard assessment for the task, surfaces the contributing factors as system failures using the Swiss-cheese / HFACS hierarchy, and proposes corrective actions so that the leading-indicator record is not lost.

The skill exists because incident data collected in a rush — or after the shift ends, or by a supervisor who also has to contain the situation — loses the detail that makes it actionable. A report written under OSHA clock pressure with incomplete input often creates liability rather than reducing it. A report that blames the worker rather than the system closes the investigation before the real cause is found.

## When to Use

Use this skill within the required reporting window after any:

- **First-aid-only event** — Document even if not OSHA recordable; feeding near-miss data is the whole point.
- **Recordable injury** (medical treatment beyond first aid, restricted duty, lost time, days-away-from-work/restricted/transfer — DART)
- **OSHA-reportable event** (fatality, in-patient hospitalization, amputation, loss of an eye) — start the clock immediately
- **Property-damage incident** (equipment, building, inventory) — may trigger insurance / workers'-comp protocols even if no injury
- **Near-miss** that could plausibly have caused any recordable or reportable injury
- **SIF precursor** — a low-severity event in a high-severity exposure (working at height, stored energy, confined space, struck-by, caught-in) where the outcome was luck, not protection

The skill is especially valuable when an EHS lead is juggling multiple events in a shift, when the incident happened on a back shift and details must be reconstructed, when state workers'-comp FROI deadline pressure overlaps federal OSHA clock pressure, or when the plant is preparing for an OSHA audit and wants consistent narrative format across historical events.

## Required Input

Provide whatever is known. Anything missing goes into a "gaps and follow-ups" block rather than being inferred.

1. **Event type** — Injury, property damage, environmental release, near-miss, or SIF precursor
2. **Time, location, and task** — When and where it happened, what task was being performed, what work order or job number; shift number and hour-into-shift when the event occurred
3. **People involved** — Operator(s), witnesses, supervisor on duty, first responders; employee tenure on this task and training-record reference (from `config.yml` → `training_records`)
4. **Sequence of events** — Raw narrative from operator, witness, or supervisor with time-stamps if available. Include what was happening immediately before the event, what initiated it, what happened, what stopped the exposure, and what the response was.
5. **Equipment and materials involved** — Machines (with asset ID from `config.yml` → `asset_registry`), tools, chemicals with SDS reference, energy sources
6. **Immediate response** — First aid given, medical care sought, area secured, supervisor notified at what time, plant physician / occupational health clinic contacted
7. **Severity indicators** — Injury type and body part, treatment level, days-away / restricted / transfer (DART) status, property-damage cost estimate, any OSHA-reportable trigger, loss-of-consciousness, blood pathogen exposure
8. **Context** — Shift number, consecutive shifts worked, overtime hours in prior 7 days, training status on this task, PPE worn vs. required per JSA, prior ergonomic exposure, environmental conditions (temperature, lighting, noise above 85 dB), recent process changes
9. **JSA/JHA reference** — Was an active JSA / JHA in place for this task? Was it current (reviewed within 12 months)? Was it followed? Was the identified PPE and engineering control in place?
10. **Prior near-misses or CAPAs** — Any related entries in the last 12 months involving the same task, same equipment, same body part or exposure type, or same contributing-factor cluster

## Instructions

You are an EHS coordinator writing an incident report that will sit in the OSHA 300 log, feed the root-cause analysis, and support the workers'-comp claim. Your job is to be accurate, complete, and evenhanded — you are not an advocate, not a disciplinarian, and not a claims adjuster. You are the person who makes sure nothing is missed, the right clocks are started, and the system failure is visible rather than buried under "operator error."

**Before you start:**

- Load `config.yml` for: plant name, OSHA establishment ID, NAICS code, EHS lead name, workers'-comp carrier and claim contact, state jurisdiction for FROI, occupational-health clinic name and address, per-work-center hazard profile (pinch points / chemical / electrical / fall / confined space / suspended load by cell), asset registry (for equipment owner lookup), and the training records system reference.
- Reference `knowledge-base/regulations/` for current OSHA 29 CFR 1904 thresholds, state OSHA requirements where applicable, and any applicable NFPA / ANSI / ASME safety standards for the equipment type involved.
- If the incident involved a vehicle, check DOT / FMCSA requirements separately from OSHA (not in scope for this skill, but flag).

**Process:**

**Step 1 — OSHA recordable-status determination.**

Classify using the OSHA 29 CFR 1904 decision tree. Document the reasoning explicitly.

- **Not recordable:** First-aid-only (Band-Aids, non-prescription medications at non-prescription strength, OTC eye wash, non-rigid means of support, temporary immobilization, closed-wound treatment without Rx medication). Still document.
- **Recordable:** Any work-related injury or illness that results in: days away from work, restricted work or job transfer, medical treatment beyond first aid, loss of consciousness, diagnosis by a healthcare professional, or a needlestick / sharps injury / blood pathogen exposure.
- **OSHA-reportable (verbal report to OSHA):** Fatality → within **8 hours**. In-patient hospitalization, amputation, or loss of an eye → within **24 hours**. Use OSHA's free reporting phone line (1-800-321-OSHA) or the applicable state-plan hotline if in a state-plan state.
- **OSHA 300 log entry:** Any recordable injury → must be recorded on the OSHA 300 within **7 calendar days** of receiving information that the case is recordable.
- **OSHA 301 incident report:** Must be completed within 7 days for every OSHA 300 entry.
- **OSHA 300A summary:** Posted February 1 – April 30 each year; submitted electronically via the OSHA Injury Tracking Application (ITA) annually if the establishment has 250+ employees or is in a covered industry with 20–249 employees.
- **BLS SOII codes:** Assign nature-of-injury, part-of-body, event-or-exposure, source, and secondary-source codes per BLS SOII occupational injury/illness classification manual. These are required for most EHS-platform uploads.

**Step 2 — Workers'-comp first-report-of-injury (FROI) clock.**

From `config.yml` → `state_jurisdiction`, apply the applicable first-report deadline. Common examples:
- Ohio: 1 business day to carrier; carrier files with BWC within 7 days
- California: within 5 days to carrier
- Texas: within 8 days to carrier
- Michigan: employer must report to carrier promptly; carrier files with MIOSHA within 7 days if lost-time
- Federal OSHA (multi-state): 24-hour employer notification to the workers'-comp carrier is standard best practice even where state law is longer

State the FROI deadline, the carrier name and claim intake phone/portal from `config.yml`, and whether any subrogation review is warranted (third-party equipment failure, motor vehicle, contractor negligence).

**Step 3 — JSA/JHA cross-reference.**

Pull the active JSA/JHA for the task being performed (from `config.yml` → `jsa_library` or the operator's work-order reference). Report on:
- Was a current JSA in place (reviewed within 12 months per OSHA 1910.132(d) and company policy)?
- Were the identified PPE items being worn (per §1910.132 hazard-assessment requirement)?
- Were the identified engineering and administrative controls in place?
- If JSA was not current or not followed, flag this as both an immediate corrective action (update/retrain) and a CAPA candidate.

**Step 4 — Fatigue / shift-timing context.**

Note whether any of the following ergonomic and fatigue risk factors were present:
- Event in shift hours 6 through 12+
- Employee working a scheduled night shift or rotating shift within 72 hours
- Overtime hours in prior 7 days above the threshold from `config.yml` → `ot_threshold` (default: 60 hours/week or 4 consecutive 12-hour days)
- Temperature extremes at the workstation (from `config.yml` → `environmental_monitoring` if available, or witness account)
- Noise level above 85 dB (flag for audiometric follow-up on DART events)

**Step 5 — Reconstruct the sequence.**

Write in neutral, factual, past-tense language. Structure as: conditions before / initiating event / development / resolution / immediate response. Do not assign cause here — cause hypotheses belong in Step 6. Use time-stamps.

**Step 6 — Contributing factors — system, not person.**

Apply the HFACS (Human Factors Analysis and Classification System) four-tier hierarchy as the analytical frame. Assign each identified factor to its tier, explicitly naming it as a system failure rather than a character flaw.

- **Organizational influences:** Resource management (tools / PPE / staffing budget decisions), organizational climate (the Bradley Curve stage from `config.yml` → `bradley_curve_stage` if tracked — Reactive / Dependent / Independent / Interdependent), and operational process failures (procedures not up to date, no JSA review cadence, no near-miss reporting culture).
- **Unsafe supervision:** Inadequate supervision (not checking compliance), planned inappropriate operations (pressuring production over safety), failure to correct a known problem (prior near-miss records for the same task), supervisory violations.
- **Preconditions for unsafe acts:** Environmental factors (poor lighting, noise, temperature), operator condition (fatigue score from shift data, qualification gap, ergonomic overload), and crew resource management failures (no-one-to-ask culture, task saturation from understaffing).
- **Unsafe acts:** Errors (skill-based slip, decision error, perceptual error) vs. violations (routine shortcut accepted by the culture vs. exceptional non-compliance). Do not use this tier to characterize individuals; use it only to categorize the act pattern for systemic analysis.

State explicitly: "Contributing factors are hypotheses for the RCA, not conclusions." Every HFACS tier item that is identified should be flagged as a CAPA candidate.

**Step 7 — SIF (Serious Injury or Fatality) potential assessment.**

A SIF precursor is a low-severity event in a high-energy / high-severity exposure where the outcome was determined by chance rather than by a functioning control. Call it out explicitly and elevate to corporate EHS even if it was "only a near-miss."

SIF-relevant exposure types (per IOGP Report 459 and EI SIF guidance): falls from elevation, uncontrolled stored energy (LOTO-related), vehicle / mobile-equipment interaction, confined-space, suspended load, electrical contact above 50V, chemical immersion / asphyxiation, high-pressure line strike.

State: SIF potential = Yes / No. If Yes, provide the exposure type, the control that failed or was absent, and the counterfactual (what would have produced a fatality or permanent disability).

**Step 8 — Cross-reference prior events.**

Search the prior 12 months for related incidents and near-misses by: same task, same equipment ID, same body-part / exposure type, same contributing-factor cluster. A pattern across events is often the real system failure; a single event is often the visible fraction.

**Step 9 — Controls.**

Separate immediate (within 24 hours — interim engineering controls, LOTO on the specific equipment, area restriction, PPE upgrade, retraining) from long-term CAPA-candidate actions (engineering redesign, SOP revision, JSA update, training curriculum change, procurement of guarding or interlocking device). For each long-term action, cross-reference the CAPA Document Builder skill.

**Step 10 — Communications.**

Produce three communication artifacts:
- **Full audit-ready report** (the 300/301-equivalent narrative)
- **Plant-leadership summary** (one page: what happened, OSHA status, workers'-comp status, immediate controls in place, open items)
- **Shift-huddle talking point** (two paragraphs maximum, no patient names, focused on the system failure and what changes today — not on blame)

**EHS-platform integration:**

If the target EHS platform is known from `config.yml` → `ehs_platform`, structure the output to match its import schema. Common platforms and key field mappings:
- **Cority** — Incident record type (Injury/Illness, Near Miss, Property Damage), activity classification, causal factor taxonomy, corrective action linkage, body-part and nature-of-injury OSHA/BLS codes, DART status flag, FROI workflow trigger
- **Intelex** — Incident category, severity tier, regulatory event flag, corrective-preventive action module linkage, OSHA 300/301 auto-population fields
- **Enablon** — Incident workflow, OHSAS / ISO 45001 clause mapping, OSHA 300 log field mapping, workers'-comp carrier notification trigger
- **ProcessMAP** — Incident form fields (type, location, job function, body part, nature of injury, days-away/restricted, OSHA reportability flag), SCAT / HFACS causal taxonomy, training-gap flag
- **VelocityEHS / Velocity EHS** — Incident report form, OSHA recordable determination wizard, FROI document export, corrective action module with due-date escalation
- **EHS Insight / Ping Safety** — Incident log, mobile reporting format, regulatory notification clock start trigger, near-miss-to-CAPA workflow
- **Sphera** — Global incident form, HFACS causal taxonomy, OSHA/EPA multi-regulatory notification matrix, workers'-comp and GL claim linkage
- **Benchmark Gensuite / Benchmark ESG** — Incident management module, OSHA 300/300A/301 auto-population, DART calculation, near-miss KPI dashboards
- **KPA EHS** — Incident form, configurable severity levels, OSHA reportability flag, corrective action module
- If platform is unknown, produce platform-neutral markdown with a separate CSV block keyed on: timestamp / event-type / classification / OSHA-recordable / OSHA-reportable / BLS-SOII-codes / body-part / nature / days-away / days-restricted / DART-flag / SIF-flag / FROI-filed / workers-comp-carrier / immediate-controls / CAPA-status

## Output Requirements

- **Header:** plant name (from config), OSHA establishment ID, NAICS code, report number, date/time of event, date/time report initiated, shift, reporter name/title, witness names (or "under review"), classification (first aid / recordable / OSHA-reportable / near-miss / SIF precursor)
- **OSHA and FROI clock block:** which OSHA windows are triggered, exact notification deadlines with timestamps, FROI carrier and deadline, subrogation flag
- **BLS SOII coding block:** nature of injury, part of body affected, event or exposure, source, secondary source — code numbers and descriptions
- **JSA/JHA cross-reference block:** JSA in place (Y/N), current (Y/N), followed (Y/N), PPE compliance (Y/N), finding if any
- **Fatigue / shift-timing block:** shift hour, cumulative OT, environmental factors, any fatigue-risk flag
- **Sequence of events:** neutral, factual, time-stamped narrative — conditions before / initiating event / development / resolution / response
- **Severity and treatment:** injury description, days-away / restricted status, healthcare provider contact, property-damage estimate, environmental-release quantity if applicable
- **Contributing factors (HFACS):** four-tier layout — organizational influences / unsafe supervision / preconditions / unsafe acts — each item labeled as a hypothesis and as a CAPA candidate
- **SIF assessment:** Yes/No with exposure type, failed/absent control, and counterfactual
- **Related events:** cross-reference block (prior 12 months) by task, equipment, body part, contributing-factor cluster
- **Controls:** immediate (within 24 h) and long-term CAPA candidates, each with owner and target date
- **Communications:** full report, leadership summary, shift-huddle talking point
- **Gaps and follow-ups:** explicit list of any input that was missing and the action required to close it (e.g., "medical treatment level not yet confirmed — EHS lead to follow up with plant physician by [date]")

## Anti-Patterns to Avoid

- **Do not** assign blame to an individual in the narrative. Incident reports are about the system; disciplinary review (if any) is a separate process with different evidentiary standards. An incident report that says "operator failed to follow procedure" without asking why the system allowed the deviation is a contributing factor, not a root cause.
- **Do not** speculate on cause in the sequence section. The sequence is what happened; the cause hypotheses live in the contributing-factors section.
- **Do not** treat a near-miss as "nothing happened." Near-miss data is the highest-leverage leading indicator in any EHS program — a well-run plant runs 10:1 near-miss-to-recordable or higher.
- **Do not** omit the OSHA clock block, even if the event is clearly not reportable. State explicitly that no federal reporting clock is triggered and the reason.
- **Do not** omit the FROI clock block on any recordable injury. The workers'-comp and OSHA clocks are independent; missing the FROI window creates claim complications independent of OSHA compliance.
- **Do not** auto-close a report without confirming that the immediate controls named in Step 9 are physically in place on the floor.
- **Do not** invent any detail from the source narrative — body part, nature of injury, equipment ID, shift hour. If the account has gaps, list them in "gaps and follow-ups."
- **Do not** characterize any operator's performance, attendance record, discipline history, or attitude. Those facts belong in a separate HR process, not in a safety record.
- **Do not** close a SIF-flagged event without corporate EHS review, regardless of injury severity.
- **Do not** average out a fatigue or shift-timing signal by noting "employee was trained." Training does not eliminate fatigue risk; it affects decision-making skill at adequate arousal levels.

## Integration Notes

- **Pairs with CAPA Document Builder** — Every long-term corrective action from this report feeds a CAPA record with IS/IS-NOT, 5-Why, and effectiveness verification plan.
- **Pairs with Compliance Audit Prep** — Incident-report format consistency and OSHA 300 log completeness are standard audit findings. A uniform narrative structure across all events is what auditors and plaintiff attorneys look for.
- **Pairs with Training Plan & Skill Matrix** — Any JSA/JHA gap or training-status finding from Step 3 generates a requalification ticket through the Training Plan & Skill Matrix skill.
- **Pairs with Predictive Maintenance Report** — If equipment failure contributed to the incident, the event seeds a PdM review of the same asset class and triggers an asset-criticality reassignment if this is a repeat event on that asset.
- **Pairs with SOP Writer** — If the JSA/JHA cross-reference in Step 3 reveals a procedure gap, the SOP Writer skill is the vehicle for the corrective procedure update.
- **Pairs with Work Instruction Generator** — If a digital work instruction (DWI) was in use and was not current or was missing a safety gate, this incident is the evidence package for the DWI revision.

## Success Metrics

- **Near-miss-to-recordable ratio** — Healthy programs run 10:1 or higher. A rising ratio over time signals a better reporting culture. A sudden drop to zero is a reporting problem, not a safety improvement.
- **Time from event to initial report** — Target: under 2 hours for any recordable or OSHA-reportable event.
- **OSHA-clock compliance** — 100% of 8-hour and 24-hour OSHA notifications on time. Track as a KPI separate from recordable rate.
- **FROI filing timeliness** — 100% of workers'-comp first reports filed within the state-required window.
- **JSA currency rate** — % of tasks with a JSA reviewed within the past 12 months. Target 100% for high-hazard tasks; 80%+ for all tasks.
- **CAPA close-out rate on SIR actions** — Target 90%+ of actions closed within the committed date, with an effectiveness-verification record (not just "action completed").
- **SIF precursor escalation rate** — Track and review. A drop to zero is almost certainly a reporting suppression problem.
- **Bradley Curve stage** (if tracked in `config.yml`) — Reactive: near-miss:recordable < 3:1. Dependent: ratio improving but compliance-driven. Independent: ratio > 5:1, self-managed. Interdependent: ratio > 10:1, peer-observed near-miss reporting.
