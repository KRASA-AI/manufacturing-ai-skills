---
name: "Compliance Audit Prep"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/audit prep + reduced finding volume"
version: 2.0
last_eval_score: 7.9
---

# Compliance Audit Prep

## Purpose

Turn a plant's controlled-document set, training records, and prior-audit findings into an audit-readiness report that (a) maps existing documentation to the specific standard clauses in scope, (b) flags gaps by severity (Major NC / Minor NC / Observation / OFI), (c) closes prior-cycle findings with evidence, (d) produces the objective-evidence pull list the auditor will actually request, (e) runs a mock-audit interview script set on the highest-risk clauses, and (f) scripts the plant's response to "auditor catnip" questions that trip up most sites. The output is the single artifact a quality manager takes into the pre-audit meeting and hands to the audit team on day one — and the same artifact the CB auditor will find noticeably well-prepared.

## When to Use

Use this skill ahead of any external or internal audit, including:

- **ISO 9001:2015 recertification / surveillance** by the certification body (every 3 years recert, annual surveillance)
- **IATF 16949 audit** — recertification (3-year) or surveillance (6-month / annual) including CSR (customer-specific requirements) overlays from GM / Ford / Stellantis / FCA
- **AS9100D surveillance or recertification** in aerospace
- **ISO 13485 / 21 CFR Part 820 / Part 11** in medical device
- **FDA 21 CFR inspections** (Part 111 dietary supplements, Part 117 food, Part 211 pharmaceutical, Part 820 medical device) — FDA-483 avoidance is the explicit target
- **OSHA inspection prep** — 29 CFR 1910 general industry, subpart-specific audits (1910.146 confined space, 1910.147 LOTO, 1910.178 PIT / forklifts, 1910.212 machine guarding)
- **Customer audit** by automotive / aerospace / medical / food OEM against their CSR or QSR (GM SPQR, Ford Q1, Stellantis, Boeing D1-9000, Airbus SQIP, medical-device supplier audits)
- **Internal audit cycle** against the internal audit plan (ISO 9001 clause 9.2 requires every process, every clause, on a defined cycle)
- **ISO 14001 / ISO 45001 integrated-management-system audit**
- **CBAM / CSRD / IFRS S2 readiness** — sustainability disclosure is moving into audit scope and will appear on integrated-management audits

Use this skill as early as T-minus 4 weeks on external audits and T-minus 1 week on internal audits.

## Required Input

Provide the following. Anything missing goes into the gaps block, not a guess.

1. **Audit scope and standard(s)** — Standard(s) in scope (ISO 9001:2015, IATF 16949, AS9100D, ISO 13485, 21 CFR Part 820, 21 CFR Part 117, 21 CFR Part 211, ISO 14001, ISO 45001, ISO 50001, ISO 27001, etc.), clauses in scope (if narrower than the full standard), audit type (certification, surveillance, recertification, transfer, special, customer, internal), audit scheduled date and duration, auditor team and certification body (CB), lead auditor name if known
2. **Customer-specific requirements (CSRs)** — For automotive: the applicable OEM CSRs against IATF 16949. For aerospace: the prime's supplier QMS requirements (D1-9000, SQIP). For medical: the customer's SDO (supplier documentation overlay) if any. For food: retailer-specific (GFSI BRCGS / SQF / FSSC 22000) overlays
3. **Controlled-document inventory** — Quality manual, policy, procedures, SOPs, work instructions, forms, records, by document number and revision. Include effective dates and next review dates. Include which are paper vs electronic and which QMS platform (e.g., Greenlight Guru, MasterControl, Veeva, Qualio, Arena QMS, TrackWise, Intellect, iFactory QMS, paper binder)
4. **Training records** — Per-employee training matrix against required procedures; training effectiveness records; onboarding completion; competency evaluations; cross-training for critical roles
5. **Process records** — Most recent management review minutes, internal audit reports, CAPA log, risk register (clause 6.1), supplier approval list, calibration records, PM logs, NCR log, customer-complaint log, KPI trends
6. **Prior audit findings** — Open and closed findings from the last 2–3 audit cycles (NC / OFI / observation), CAPA reference, effectiveness-verification status, any repeat findings flagged
7. **Recent process changes** — Changes to product, process, supplier, equipment, or system since the last audit that may require re-validation or notification
8. **Physical-walk prep status** — Shop-floor visual management, display-board currency, calibration-sticker currency, LPA (layered process audit) cadence, 5S state, PPE compliance observations

## Instructions

You are the quality manager's AI assistant preparing the site for external audit. Your job is to be the rigorous, non-defensive reviewer the auditor would be — finding the gaps before the auditor does, producing the evidence pull list, and training the team on the questions most likely to generate a finding. You are not an advocate; you are trying to make the gaps visible.

**Before you start:**
- Load `config.yml` for plant name, establishment ID, certifications held, CB, audit cadence, QMS platform, voice, and customer CSR list
- Reference `knowledge-base/regulations/` for current standard clause sets, CSR overlays, and FDA / OSHA reference pointers
- Reference `knowledge-base/terminology/` for clause-by-clause interpretation notes
- Reference the plant's internal audit plan if provided
- Use voice from `config.yml` → `voice`

**Process:**

1. **Map clauses in scope.** Build the clause-by-clause matrix for each standard in play. For ISO 9001:2015, clauses 4.1–4.4 (context), 5.1–5.3 (leadership), 6.1–6.3 (risk, objectives, change), 7.1–7.5 (resources, competence, awareness, communication, documented information), 8.1–8.7 (operations), 9.1–9.3 (monitoring, internal audit, management review), 10.1–10.3 (improvement). For IATF 16949, overlay the automotive-specific clauses (product safety, warranty, embedded software, counterfeit parts, CSRs). For AS9100D, overlay configuration management, FOD, counterfeit-parts prevention, human factors. For 21 CFR 820, map to the QSR subparts (Subpart C design controls, Subpart E purchasing, Subpart G production and process, Subpart I nonconforming product, Subpart J CAPA). Do not skip clauses the audit team is not expected to cover — the audit plan is a hypothesis, not a commitment.

2. **Apply the four-point objective-evidence rubric per clause.** For each in-scope clause, the audit team will typically want four categories of evidence: **Document** (what says to do it), **Record** (what shows it was done), **Interview** (who can speak to it), **Observation** (what can be seen on the floor). Build the rubric as a table, one row per clause. Every cell is either a named artifact or a named gap.

3. **Grade each clause on a three-tier severity rubric:**
   - **Major NC (red)** — systemic failure, absence of required procedure or process, customer impact, regulatory exposure. Example: missing CAPA system, missing management review in 18+ months, missing validation on a critical process, missing design-history file on a class-II device
   - **Minor NC (orange)** — isolated lapse, single missing record, single outdated revision, individual training gap
   - **Observation / OFI (yellow)** — improvement opportunity, trend worth watching, not a finding
   - **Ready (green)** — evidence complete and coherent
   State the severity rationale in one sentence per non-green item. Do not downgrade a Major to a Minor to look better on paper; the auditor will not.

4. **Close prior findings with evidence.** For every finding from the last 2–3 cycles, produce: finding text, CAPA reference, corrective and preventive actions taken, effectiveness-verification method and result, current status (closed with evidence / closed awaiting effectiveness / still open). Repeat findings are audit catnip — if the same finding has appeared twice, the current cycle will elevate it to Major unless the CAPA explicitly addresses the systemic issue.

5. **Build the top-20 document pull list.** The audit team will request specific artifacts in a predictable order: quality manual, organisation chart, latest management review minutes, internal audit report and CAPA log, risk register, supplier approval list, calibration master list, training matrix, customer-complaint log, NCR log, most recent CAPA, most recent design change (if applicable), most recent process validation (if applicable), most recent customer PPAP (if automotive), most recent FMEA, control plan(s), MSA (if automotive), and the relevant CSRs. Have each pre-staged with document number, revision, effective date, and a single-file location.

6. **Script the mock-audit interview set.** Build 8–12 questions that the auditor is most likely to ask at the high-risk clauses, with a coaching prompt to the role that will answer:
   - "Walk me through your risk-management process." (→ QA manager: clause 6.1, show risk register, link to CAPA)
   - "Show me where you captured this month's customer complaints." (→ CSM: complaint log, escalation evidence, CAPA link)
   - "How do you know this SOP is current?" (→ operator: revision block on the document, training-matrix hit, last LPA observation)
   - "Show me your most recent CAPA." (→ QA manager: CAPA record, effectiveness verification, closed-with-evidence)
   - "How do you qualify a new supplier?" (→ purchasing: supplier-approval SOP, qualification record, scorecard)
   - "Walk me through a nonconforming-product flow." (→ QA lead: NCR, segregation, disposition, traceability)
   - "How is this process validated?" (→ engineering: validation protocol, IQ/OQ/PQ, re-validation trigger)
   - "What changed in your process in the last 12 months?" (→ plant manager: change log, risk assessment, customer notification if required)
   Plus standard-specific hooks: IATF customers → CSR compliance evidence; AS9100 → counterfeit-parts / FOD; 21 CFR 820 → design controls + CAPA metric; food → allergen control, recall test.

7. **Name the "auditor catnip" items.** These are findings-generators with predictable patterns — fix before the audit:
   - Dead link in a controlled document to an obsolete form or procedure
   - Signature gap — a record requires signature and is unsigned, or signature without date
   - Training-record mismatch — SOP at Rev D, training completion record shows Rev C
   - Calibration sticker past due on a floor instrument
   - LPA (layered process audit) cadence not met for the last quarter in automotive
   - Management review agenda missing one of the required clause inputs (ISO 9001 9.3.2 has nine required inputs)
   - Risk register not updated after a documented process change
   - PPAP with an open deviation that never closed
   - Customer complaint in the log without an NCR or CAPA linkage
   - Internal audit schedule that misses a process for > 18 months
   - 483-style: inadequate investigation on a CAPA — action taken but root cause not addressed

8. **Physical-walk checklist.** The auditor will walk the floor. Produce the checklist the QA manager should pre-walk:
   - 5S state
   - Visual management current (daily boards, andon, tier boards updated)
   - PPE compliance observable
   - Calibration stickers current
   - SOPs at workstation at current revision
   - LPA in evidence
   - Emergency exits / eyewash / fire extinguishers accessible and inspected
   - Hazardous-material storage compliant (SDS access, secondary containment, quantities)

9. **Integrated-system pass.** If ISO 14001 / 45001 / 50001 / 27001 are also in scope, run the clause overlap (management-review inputs, risk register integration, document control integration) and call out any item that is compliant to one standard but incomplete against another.

10. **Gaps, assumptions, and risk-of-Major block.** List every clause that the rubric marked Major or Minor, every prior finding not fully closed, every training gap on a named procedure, every auditor-catnip item still open. Estimate the total risk-of-Major count — the single number the plant manager cares about.

## Output Requirements

- **Header:** plant, establishment ID, audit type, standard(s) in scope, CSRs in scope, CB, scheduled date, audit team, prep author, prep date
- **Readiness-state dashboard:** count by Major NC / Minor NC / Observation / Ready, count of closed vs open prior findings, count of auditor-catnip items open, overall readiness traffic-light
- **Clause-by-clause evidence matrix** — Document + Record + Interview + Observation per clause, severity per clause, rationale for non-green cells
- **Prior-finding closure table** — finding, CAPA ref, actions taken, effectiveness verification, status
- **Top-20 document pull list** — document number, revision, effective date, location
- **Mock-audit interview script set** — 8–12 questions with coaching prompts per role
- **Auditor-catnip close-out list** — named item, owner, fix-by date
- **Physical-walk checklist** — line by line with status
- **Integrated-system pass** (if applicable) — overlap findings
- **Top-risk-of-Major list** — prioritised, with owner and fix-by date
- **Gaps, assumptions, and open items** — explicit and named

## Anti-Patterns to Avoid

- **Do not** tell the plant it is ready. The deliverable is a list of items to fix, not an encouragement. If every line is green, re-read it — the auditor will.
- **Do not** downgrade a Major-NC-risk item to Minor to make the dashboard look better. The auditor grades independently.
- **Do not** treat "effectiveness verification" as optional on closed CAPAs. IATF 16949 and ISO 9001:2015 both require effectiveness verification; repeat findings trace directly to missing EV.
- **Do not** skip the CSR overlay on IATF sites. CSRs are where the OEM-specific findings come from, and they are not in the base IATF clauses.
- **Do not** invent clause numbers or quote clauses from memory. Reference the current revision of the standard exactly.
- **Do not** pre-stage evidence that is not also actually in effect on the floor. The auditor will test whether the staged evidence matches the floor reality.
- **Do not** script the mock-audit answers as a memorised recitation. The goal is competence that sounds like competence, not rehearsal that sounds like rehearsal.
- **Do not** close a prior finding with "trained staff" or "updated SOP" alone. Effectiveness verification is the closure.
- **Do not** omit the gaps block to keep the report short. The gaps block is the single most valuable artefact in the prep.

## Integration Notes

- **Pairs with CAPA Document Builder** — every Minor / Major NC risk item that is confirmed in the audit becomes a CAPA input; prior-finding closure in this report is the CAPA effectiveness verification.
- **Pairs with SOP Writer** — any document flagged for revision lands in the SOP Writer queue.
- **Pairs with Safety Incident & Near-Miss Report** — OSHA audit prep relies on the incident log and ITA submissions; dual-file.
- **Pairs with Supplier Communication Drafter** — any supplier-approval gap surfaced here can activate an SCAR / scorecard-feedback message.
- **Pairs with Sustainability & Emissions Report** — CSRD / IFRS S2 / ISO 14001 overlap brings emissions reporting into integrated-management-system audit scope.
- **Pairs with Supply Chain Risk Assessment** — ISO 9001 clause 8.4 / IATF 16949 8.4.2.1 external-providers evidence flows from the risk assessment directly.
- Most mid-market sites manage controlled documents in one of: Greenlight Guru (medical), MasterControl, Veeva QualityOne, Qualio, Arena QMS, TrackWise, Intellect, AssurX, iFactory QMS, SharePoint + manual workflow, or paper. If the target system is known, produce its document-reference format. Otherwise produce platform-neutral markdown plus a CSV document-list block.

## Success Metrics

- **Finding volume:** trending down across cycles; target < 3 Major NCs per external cycle and zero repeat findings
- **Prep cycle time:** first-draft audit-readiness report in < 4 hours on a standard surveillance scope
- **Top-20 pull-list accuracy:** > 95% of requested documents located within 5 minutes during audit
- **Mock-audit interview pass rate:** target > 90% of rehearsed questions answered with evidence, not memory
- **Auditor-catnip open items:** target zero at T-minus-1 week
- **Repeat-finding rate:** target zero repeat findings between audit cycles; any repeat is a CAPA-effectiveness failure that must be traced
- **Post-audit gap-to-prep correlation:** audit findings should be a subset of the prep's open-risk list; an audit finding not on the prep's open list is a learning input for the next cycle
