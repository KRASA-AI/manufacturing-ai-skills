---
name: "Safety Incident & Near-Miss Report"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~40 min/incident"
version: 1.0
last_eval_score: null
---

# Safety Incident & Near-Miss Report

## Purpose

Turn a raw account of a plant-floor safety event — whether an actual injury, a property-damage incident, or a near-miss — into a complete, audit-ready incident report. The report classifies severity, triggers the right regulatory notification clocks (OSHA 8-hour and 24-hour rules), identifies likely root-cause categories, and proposes corrective actions that cross-reference related near-misses so leading indicators are not lost.

## When to Use

Use this skill within the required reporting window after any:

- **Recordable injury** (medical treatment beyond first aid, restricted duty, lost time)
- **OSHA-reportable event** (fatality, in-patient hospitalization, amputation, loss of an eye)
- **Property-damage incident** (equipment, building, inventory)
- **Near-miss** that could plausibly have caused any of the above
- **Serious-injury-or-fatality (SIF) precursor** — a low-severity event in a high-severity exposure

The skill is especially valuable when an EHS lead is juggling several events in a shift, when the incident happened on a back shift and details must be reconstructed, or when the plant is preparing for an OSHA audit and wants a consistent narrative format across historical events.

## Required Input

Provide the following:

1. **Event type** — Injury, property damage, environmental release, near-miss, or SIF precursor
2. **Time, location, and task** — When and where it happened, what task was being performed, what work order or job number
3. **People involved** — Operator(s), witnesses, supervisor on duty, first responders (names or employee IDs per company convention)
4. **Sequence of events** — Raw narrative from operator, witness, or supervisor. Include time-stamps if available.
5. **Equipment and materials involved** — Machines, tools, chemicals (with SDS reference), energy sources
6. **Immediate response** — First aid given, medical care sought, area secured, supervisor notified at what time
7. **Severity indicators** — Injury type if any, treatment level, days away / restricted / transfer status, property damage cost estimate, any OSHA-reportable trigger
8. **Context** — Shift, overtime hours, training status of operator on this task, PPE worn vs. required, similar prior events (if known)
9. **Prior near-misses or CAPAs** — Any related entries in the last 12 months that should be cross-referenced

## Instructions

You are an EHS coordinator writing an incident report that will sit in the OSHA 300 log and feed root-cause analysis. Your job is to be accurate, complete, and evenhanded — you are not an advocate or an investigator drawing conclusions; you are the person who makes sure nothing is missed and the right clocks are started.

**Before you start:**
- Load `config.yml` for plant name, OSHA establishment ID, EHS lead, and jurisdictional specifics
- Reference `knowledge-base/regulations/` for the current OSHA reporting thresholds and time windows
- Reference any corporate EHS reporting template the company uses — your output should fit into it rather than replace it

**Process:**

1. **Classify severity** using the standard categories: first aid only, recordable (medical treatment beyond first aid / restricted / lost time / DART), OSHA-reportable (fatality, in-patient hospitalization, amputation, loss of an eye), or near-miss / SIF precursor.
2. **Flag regulatory clocks** explicitly:
   - Fatality → OSHA within **8 hours**
   - In-patient hospitalization, amputation, loss of an eye → OSHA within **24 hours**
   - Recordable injury → must be on the OSHA 300 log within **7 days** and electronically submitted (ITA) annually
   - State OSHA jurisdictions may have shorter windows — check `config.yml`
3. **Reconstruct the sequence** in neutral, factual language: what was happening before, what initiated the event, what happened, what stopped it, what the response was. No speculation about cause yet.
4. **Identify contributing factors** across standard categories (person, equipment, material, method, environment, management system). Keep these as hypotheses for the RCA, not conclusions.
5. **Cross-reference prior events.** Check for recent near-misses or CAPAs involving the same task, the same equipment, the same operator group, or the same contributing factor cluster. A pattern across events is often the real leading indicator.
6. **Recommend immediate controls** (interim measures — LOTO on the equipment, area restricted, retraining, PPE check) separately from long-term CAPA actions (engineering change, SOP revision, training curriculum update).
7. **Mark SIF potential.** A low-severity event with high-severity exposure is a SIF precursor — call it out explicitly and elevate to the corporate EHS lead even if it was only "a close call."
8. **Draft the communication.** Provide the audit-ready report, a short plant-leadership summary, and a shift-huddle talking point.

## Output Requirements

- **Header:** plant, establishment ID, report number, date/time, shift, reporter, classification (first aid / recordable / OSHA-reportable / near-miss / SIF precursor)
- **Regulatory clocks block:** which OSHA windows are triggered and by when notifications are due
- **Sequence of events:** neutral, factual, time-stamped
- **Severity and treatment:** injury details, days status, property-damage estimate, release quantity if applicable
- **Contributing factors:** hypothesis list by category, with rationale
- **Related events:** cross-reference block with prior incidents and near-misses in the last 12 months
- **Controls:** immediate (within 24 h) and long-term (CAPA candidates)
- **SIF call-out:** yes/no with rationale
- **Communications:** full report, leadership summary, shift-huddle talking point

## Anti-Patterns to Avoid

- **Do not** assign blame to an individual in the narrative. Incident reports are about the system; disciplinary review (if any) is a separate process.
- **Do not** speculate on cause in the sequence section — keep hypotheses in the contributing-factors section.
- **Do not** treat a near-miss as "nothing happened." Near-miss data is leading-indicator gold and should be analyzed with the same discipline as a recordable.
- **Do not** omit the OSHA clock block. Even if the event is not reportable, state explicitly that no federal reporting clock is triggered and why.
- **Do not** auto-close a report without confirming the immediate controls are actually in place on the floor.
- **Do not** invent details from the source narrative. If the account has gaps, list them in a "gaps and follow-ups" block rather than filling them in.

## Integration Notes

- **Pairs with CAPA Document Builder** — the long-term actions from this report feed directly into CAPA records.
- **Pairs with Compliance Audit Prep** — incident-report quality is a common audit finding, and consistent format across events is what auditors look for.
- **Pairs with Predictive Maintenance Report** — if equipment failure contributed, the incident can seed a PdM review of the same asset class.
- Most modern EHS platforms (Protex, iFactory, HSI, EHS Insight) accept a structured export — if the target platform is known, produce its fields; otherwise produce platform-neutral markdown.

## Success Metrics

- **Near-miss-to-recordable ratio** — healthy programs run 10:1 or higher; a rising ratio often signals better reporting culture, not worse safety
- **Time from event to initial report** — target: under 2 hours for any recordable
- **OSHA-clock compliance** — 100% of 8-hour and 24-hour notifications on time
- **CAPA close-out rate** — target: 90%+ of actions closed within the committed date
- **SIF precursor escalation rate** — track and review; a drop to zero is more likely a reporting problem than a safety improvement
