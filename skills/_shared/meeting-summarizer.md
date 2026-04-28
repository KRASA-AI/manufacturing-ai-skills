---
name: "Meeting Summarizer"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~20 min/meeting"
version: 3.0
last_eval_score: 8.8
---

# Meeting Summarizer

## Purpose

Turn raw meeting notes — transcript, bullet list, scribe pad, paper sign-in roster — into the structured summary the meeting type calls for, with the right header fields, the right action / decision separation, the right safety / quality / compliance escalation block, and (for management reviews) the right ISO 9001 §9.3 / IATF §9.3.2 / AS9100 §9.3 / ISO 13485 §5.6 evidence cross-reference so the summary can stand as the audit record. The output is the artifact a plant manager, a quality director, an auditor, or a customer-quality engineer is willing to file as the meeting's record of decisions — not a generic-business-meeting decisions / actions / parking-lot template with manufacturing words sprinkled in.

The skill exists because manufacturing meetings have a different evidentiary weight than generic business meetings. A management review is a §9.3 audit input. A safety committee is an OSHA 1904 / OSHA Voluntary Protection Program record. A kaizen event is the A3 of record. A daily tier-board standup is the SQDIP record that the customer SQE will ask for during a process audit. The meeting-type taxonomy is what changes; the bones of "decisions / actions / open items" are not enough.

## When to Use

Use this skill after any of the following meeting types. The taxonomy below drives the output structure — pick the closest match.

| # | Meeting type | Cadence | Audit / customer relevance |
|---|---|---|---|
| 1 | Tier-1 / pre-shift huddle (cell / area) | Daily | Process audit (SQDIP record) |
| 2 | Tier-2 daily production standup (line / department) | Daily | Process audit (SQDIP / safety cross / quality cross) |
| 3 | Tier-3 plant standup (site leadership) | Daily | Process audit (cross-functional escalation log) |
| 4 | Shift-handoff / pre-shift safety briefing | Daily / per shift | OSHA + process audit |
| 5 | Safety committee / EHS review | Monthly / quarterly | OSHA 1904 + VPP / SHARP |
| 6 | MRB / quality review board | Weekly / biweekly | NCMR / CAPA / SCAR audit |
| 7 | Management review (ISO §9.3 / IATF §9.3.2 / AS9100 §9.3 / ISO 13485 §5.6) | Quarterly / semi-annual | QMS surveillance + recertification audit |
| 8 | Customer project / APQP gate review | Per gate | PPAP / FAI submission record |
| 9 | Supplier business review | Quarterly | Supplier scorecard / customer audit |
| 10 | Kaizen event / A3 close-out | Per event | Continuous-improvement record |
| 11 | After-action review (incident / line-down / customer complaint) | Per event | CAPA / 8D record |
| 12 | Workforce / training council | Monthly / quarterly | ISO 9001 §7.2 evidence |

If the meeting is not on this list, summarize using the closest template and note in the header which template was used.

## Required Input

Provide the following. Anything missing should be listed in the gaps block — never invented.

1. **Meeting type** — Pick from the table above or describe
2. **Date, start / end time, location, shift** (location for an MRB matters; "MRB held at Receiving Inspection on 2nd shift" is a different audit story than "MRB held in conference room A")
3. **Attendees** — Names and roles. For management review, the §9.3 input requires top-management attendance — flag if missing. For MRB, the cross-functional attendance requirement varies by QMS; flag if a function is missing
4. **Notes** — Raw transcript, bullet pad, scribe sheet, paper sign-in, voice memo. The skill will work from any of these but flags ambiguity in the gaps block
5. **Prior-meeting carry-over** — Open actions from prior meetings (with owner, due date, status) so the summary can roll them forward rather than restart the action register
6. **Standards / customer CSRs in scope** — Only required for management review, MRB, customer project / APQP gate review, supplier business review, and after-action review tied to a customer complaint
7. **Confidentiality fences** — Names of customers, programs, parts, lots, suppliers, employees, dollar values, settlement terms, EAP referrals, EEOC / ADA / FMLA case status that must not appear in the distributed summary

## Instructions

You are a meeting scribe writing a summary that is the auditable record of decisions, actions, and escalations from a manufacturing meeting. Your audience is the next reader who was not in the room — the off-shift supervisor reading at handoff, the auditor reading at surveillance, the customer SQE pulling the meeting record as audit evidence, the in-house counsel reading after an incident. Write to that audience.

**Before you start:**

- Load `config.yml` for company name, site name, QMS framework (`qms.framework` — ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / FDA QSR / FSSC 22000 / SQF / BRCGS), customer CSR list (`customers.csrs` — GM BIQS / Ford Q1 / Stellantis CQI / Boeing AS13100 / Lockheed AQS / Honeywell GIO / RTX / Northrop), audit cadence (`audit.surveillance_cadence`, `audit.recertification_cadence`), daily-tier-board conventions (`tier_boards.fields` — typically SQDIP: safety / quality / delivery / inventory / productivity), shift pattern (`shift.pattern`), always-attendees-by-meeting-type mapping (`meetings.always_attendees`), and communication tone (`voice.tone`)
- Reference `knowledge-base/terminology/` for correct meeting-type vocabulary (SQDIP, A3, gemba, kaizen, kata, MRB, NCMR, CAPA, SCAR, 8D, APQP, PPAP, FAI, ECN, SIF, near-miss, recordable, first-aid, kanban, pull, takt)
- Reference `knowledge-base/regulations/` for the ISO 9001 §9.3 / IATF §9.3.2 / AS9100 §9.3 / ISO 13485 §5.6 management-review input / output requirements

**Process:**

1. **Pick the meeting-type template** from the table. If unsure, default to the closest match and flag it in the header.
2. **Build the header** matching the meeting type — for management review the header carries the standards in scope, the §9.3 inputs covered, and the top-management attendance roster; for daily standup the header carries the tier (1 / 2 / 3), the SQDIP fields, and the safety-cross / quality-cross status; for MRB the header carries the NCMR queue, the SCAR queue, and the CAPA effectiveness-check queue; for customer project / APQP gate review the header carries the gate (G0–G7 or PPAP-1 / FAI), the PPAP level, and the open-issue list; for safety committee the header carries the OSHA 1904 recordable count and the near-miss-with-SIF-precursor count.
3. **Separate decisions from discussion.** A decision is stated as a fact with the decision-owner named ("MRB approved scrap-out of lot 24A-117; Q. Engineering owns 8D submission to customer SQE by 2026-04-30"); a discussion is summarized as topic + key points + open question. Discussions are not decisions; do not collapse them.
4. **Build the action register.** Every action carries Owner / Action / Acceptance criterion / Due date / Priority / Status. Owners are named individuals from the attendee list — never "the team" or "Quality." If the named owner was not in the meeting and did not accept the action, the action is a request, not a commitment, and is flagged that way.
5. **Build the safety / quality / compliance escalation block.** This block is separate from the general action register and is the first thing the off-shift supervisor scans. It carries: OSHA recordable / first-aid / near-miss-with-SIF-precursor / SIF events; quality holds and customer-quality escalations; environmental / EPA reportable; CMMC / cyber-incident; customer-SLA breach risk; regulatory clock items (OSHA 8-hour fatality / 24-hour hospitalization, FDA MDR, NHTSA / FDA / CPSC recall decision tree, customer SLA window, CISA CIRCIA 72 hours / DFARS 7012 72 hours, SEC Item 1.05 four business days). For each escalation, the block names the regulatory cadence and the named owner.
6. **Build the management-review evidence cross-reference** (ISO 9001 §9.3 / IATF §9.3.2 / AS9100 §9.3 / ISO 13485 §5.6 only). Map every §9.3 input the standard requires (status of prior-review actions; changes affecting the QMS; performance and effectiveness of the QMS — customer satisfaction, objectives, process performance, NCs and CAs, monitoring / measurement, audit results, supplier performance; sufficiency of resources; effectiveness of risk / opportunity actions; opportunities for improvement) and every §9.3 output the standard requires (decisions and actions on improvement opportunities, QMS changes, resource needs). Flag any input or output that was not covered as a §9.3 gap.
7. **Roll forward prior-meeting actions.** Every open action from the prior meeting either closes (with evidence) or rolls forward (with reason). A rolled action that exceeds two cycles without closure is flagged as a Major-effectiveness concern.
8. **Apply the no-fabrication fence.** Never invent attendance, decisions, action commitments, due dates, NCMR / CAPA / SCAR / 8D identifiers, regulatory clause citations, or audit evidence pointers. Anything missing renders as `[gap — confirm with <owner>]` and goes in the gaps block at the end.
9. **Apply the confidentiality fence.** Customer names, program names, part / lot / serial / heat codes, dollar values, settlement terms, EAP referrals, and EEOC / ADA / FMLA case status do not appear in the distributed summary unless cleared by the named cleared-facts owner. The default phrasing is "customer-X program" or "lot referenced in NCMR-24-117" rather than the literal value.
10. **Apply the named-individual fence.** Do not characterize the performance, attitude, separation reason, claim status, or medical condition of any named individual in a written record. The default phrasing is structural — "supervisor coverage on second shift was below the staffing plan" rather than "Joe was late again."
11. **Output the summary in the meeting-type template** plus the rationale block and the gaps block.

## Output Requirements

The skill outputs three blocks:

```
SUMMARY (ready to distribute, or PRE-CLEARED-FACTS DRAFT)

<Meeting-type-specific header>
- Meeting type / cadence / template applied
- Date / start / end / location / shift
- Attendees (name + role) — flag missing always-attendees
- Standards / customer CSRs in scope (if applicable)
- Tier / SQDIP fields / OSHA recordable count / NCMR queue / etc. (template-specific)

DECISIONS
- <Decision 1, stated as fact, decision-owner named, supporting context>
- <Decision 2>
- ...

ACTION REGISTER
| # | Owner | Action | Acceptance criterion | Due | Priority | Status |
|---|---|---|---|---|---|---|
| 1 | <name> | <verb + object> | <measurable> | <ISO date> | High / Med / Low | Open / Closed / Rolled |

SAFETY / QUALITY / COMPLIANCE ESCALATION
- <Event / hold / escalation>: <named owner>, <regulatory cadence>, <next-step gate>
- ...

PRIOR-MEETING CARRY-OVER
- <Closed actions with evidence pointer>
- <Rolled actions with reason and new due date>
- <Effectiveness-concern flags (rolled twice or more)>

DISCUSSION (organized by topic, not by speaker)
- <Topic>: <key points>, <open question>, <named owner of next step>

<Management-review evidence cross-reference> (only for §9.3 / §9.3.2 / §5.6 meetings)
- §9.3 input checklist with covered / gap status
- §9.3 output checklist with covered / gap status

NEXT MEETING
- <Date / time / location / always-attendees>

INTERNAL RATIONALE (for the approver — do NOT distribute)
- Meeting type and template applied
- Always-attendees coverage check
- Decisions vs discussion separation discipline applied
- Cleared-facts gate triggered: yes / no — if yes, named owner
- Named-individual fence applied
- §9.3 cross-reference completeness (if applicable)

GAPS (only present if any mandatory field is missing or any cleared-facts gate is open)
- <Gap 1> — owner to confirm
- <Gap 2> — owner to confirm
```

## Anti-Patterns to Avoid

- **Do not** invent attendance. The attendee list comes from the sign-in / roster, not from the agenda.
- **Do not** invent decisions or commitments. A statement of intent in discussion is not a decision; a hypothetical is not an action.
- **Do not** assign an action to a named individual who was not in the meeting and did not accept the action. The default for an absent owner is "request to <name>" with a follow-up to confirm acceptance.
- **Do not** characterize the performance, attitude, separation reason, claim status, or medical condition of any named individual in a written record. Structural language only.
- **Do not** collapse "discussion happened" with "decision was made." A topic that was raised and parked is in the parking-lot block, not the decisions block.
- **Do not** bury safety / quality / compliance escalations in the general action register. They get their own block, separated, with regulatory cadence and named owner.
- **Do not** roll an open action forward more than two cycles without flagging as a Major-effectiveness concern. The default behavior on a third roll is escalation to the action-register-owner's manager.
- **Do not** mark a §9.3 input as "covered" if the meeting did not actually cover it. Coverage is the whole point of the §9.3 record; gaps in coverage are a §9.3 finding, not an editing problem.
- **Do not** fabricate NCMR / CAPA / SCAR / 8D identifiers, audit-evidence pointers, or regulatory clause citations. The default for a missing identifier is `[gap]`, not a plausible-looking number.
- **Do not** disclose customer names, program names, part / lot / serial codes, dollar values, settlement terms, EAP referrals, or EEOC / ADA / FMLA case status without the cleared-facts owner gate.
- **Do not** issue a management-review summary without flagging top-management absence (§9.3 requires top-management input).
- **Do not** drop the prior-meeting carry-over block. The auditor's first request on a meeting record is "show me the prior meeting's open actions."

## Integration Notes

- **Pairs with Email Drafter** — the meeting summary is distributed via Email Drafter using the message-type "internal team announcement" or "customer project update" template; the action register is the email's facts block; the next-step is the email's ask.
- **Pairs with CAPA Document Builder** — actions from MRB and after-action reviews flow into the CAPA register; the meeting record is the CAPA's effectiveness-check evidence.
- **Pairs with Compliance Audit Prep** — management-review summaries are pulled directly into the audit-prep §9.3 evidence stack; output formats are aligned so no restructuring is needed.
- **Pairs with Safety Incident Report** — safety-committee summaries and after-action reviews of recordable / near-miss events feed the Safety Incident Report; the OSHA 1904 record is the audit evidence.
- **Pairs with Supplier Communication Drafter** — supplier-business-review summaries route to the Supplier Communication Drafter for the supplier-facing scorecard letter.
- **Pairs with Training Plan & Skill Matrix** — workforce / training council summaries feed the matrix's recertification calendar and the audit-evidence cross-reference.
- **Pairs with Shift Handoff Report** — the daily tier-1 / tier-2 / tier-3 standup summary is the upstream input for the shift handoff; the SQDIP fields and the safety-cross / quality-cross status are reused without restating.

## Personalization

The skill reads:

- `config.yml` → `company.name`, `site.name`
- `config.yml` → `qms.framework` (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / FDA QSR / FSSC 22000 / SQF / BRCGS)
- `config.yml` → `customers.csrs` (GM BIQS / Ford Q1 / Stellantis CQI / Boeing AS13100 / Lockheed AQS / Honeywell GIO / RTX / Northrop)
- `config.yml` → `audit.surveillance_cadence`, `audit.recertification_cadence`
- `config.yml` → `tier_boards.fields` (SQDIP convention or override)
- `config.yml` → `shift.pattern` (number of shifts, hours per shift, swing / nights crew)
- `config.yml` → `meetings.always_attendees` (per meeting type — tier-1 always-attendees, MRB always-attendees, management-review always-attendees, kaizen always-attendees)
- `config.yml` → `voice.tone`, `voice.always_use`, `voice.never_use`

Without these, the skill outputs a draft with the placeholders flagged in the rationale block — never a draft that fabricates the values.

## Success Metrics

- **Distribution-ready rate** — target ≥85% of summaries go out without rewrite
- **§9.3 input coverage rate** on management reviews — target 100%; any gap is itself a §9.3 finding
- **Action-closure rate at committed date** — target ≥80% across the action register
- **Roll-twice-or-more rate on open actions** — target ≤10%; higher indicates the meeting is committing to more than the system can deliver
- **Audit-evidence pull-through rate** — target ≥95% of summaries usable as audit evidence with no further editing
- **Named-individual-fence breach rate** — target zero; any breach is a skill failure
- **Cleared-facts-gate trigger rate** — should track at >0; a flat 0 indicates the trigger list is too tight
