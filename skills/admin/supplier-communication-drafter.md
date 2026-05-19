---
name: "Supplier Communication Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/email"
version: 3.0
last_eval_score: 9.0
---

# Supplier Communication Drafter

## Purpose

Draft professional, contractually-aware supplier communications across the full PO-to-payment lifecycle — PO confirmations and expedites, Supplier Corrective Action Requests (SCARs / 8D requests), delivery escalations, quality holds, customer-driven controlled-shipping cascades (Ford CSL-1 / CSL-2, GM CS-I / CS-II, Stellantis NCT, Toyota CL2 / CL3, Honda QAS escalation), Section 232 / Section 301 / Section 232 pharmaceutical tariff cost pass-through requests, RFQ follow-ups, scorecard feedback, PPAP submission requests, end-of-life and last-time-buy notices, and supplier-audit notices — in the plant's voice, with the right level of firmness for the situation, the supplier-tier relationship, and the governing quality-agreement clause.

The Supplier Communication Drafter is the external-facing counterpart to the Quality Report Generator and Supply Chain Risk Assessment. When an internal quality finding (NCR / CAPA / vision-alarm cluster) crosses the plant boundary, this skill produces the written record that holds up in a future scorecard review, registrar audit, customer audit, or commercial dispute.

## When to Use

Use this skill whenever you need to send a written communication to a supplier that is higher-stakes than a casual "can you check on this" — in particular when the message will be referenced in a future audit, scorecard review, deviation, dispute, customer-driven cascade, or regulatory inquiry. Specific triggers:

- PO acknowledgment overdue beyond the quality-agreement window
- Incoming lot on quality hold pending supplier response
- Late delivery with line-stop risk or customer-expedite impact
- SCAR opening, SCAR response review, or SCAR close-out
- Annual or quarterly scorecard conversation (per IATF 16949 §8.4 supplier-performance requirement)
- Pricing or PPV dispute, or supplier-initiated price-change request
- A first warning before escalation to procurement leadership or the supplier's executive sponsor
- Customer-imposed controlled shipping has cascaded to your operation and must cascade further upstream (Ford CSL-1 → CSL-2 → revoked Q1, GM CS-I → CS-II → New Business Hold, Stellantis NCT, Toyota CL2 → CL3, Honda QAS escalation, Boeing controlled-shipping per D6-83107)
- Tariff cost pass-through under Section 232 metals (steel and aluminum derivatives at 50% effective 2025-08-18; Annex IV technical correction note 2026-05-06), Section 232 pharmaceutical onshoring (100% effective 2026-07-31 large companies / 2026-09-29 small companies, with onshoring-agreement application window 2026-05-11 → 2026-06-12), Section 232 semiconductor Phase 2 (data-center semiconductor market update due 2026-07-01 per Proclamation 11002), or Section 301 China-origin lines
- PPAP submission request or PPAP discrepancy callout (AIAG PPAP 4th ed. or customer-specific Level 1–5 submission)
- Deviation request review (incoming supplier deviation; outgoing customer deviation)
- End-of-life (EOL), product-discontinuance, or last-time-buy (LTB) notice from supplier
- Supplier audit notice (on-site, virtual, or process-audit per VDA 6.3, AIAG CQI-9 heat treat, CQI-11 plating, CQI-12 coating, CQI-15 welding, CQI-17 soldering, CQI-23 molding, CQI-27 casting)
- Supplier-portal exception (Ariba / Coupa / Jaggaer / Oracle Procurement / SAP SLC) requiring out-of-band confirmation
- Conflict-minerals / UFLPA / EUDR / Modern-Slavery / REACH / RoHS / CRMA compliance attestation request

## Required Input

Provide the following. Mark anything unknown as "TO BE COMPLETED" rather than guessing.

1. **Message type** — Pick from: `po-confirmation`, `po-expedite`, `scar-8d-request`, `scar-response-review`, `delivery-escalation`, `quality-hold-notice`, `controlled-shipping-cascade`, `tariff-pass-through-request`, `ppap-submission-request`, `rfq-followup`, `scorecard-feedback`, `price-dispute`, `deviation-request-review`, `eol-ltb-notice-acknowledgment`, `supplier-audit-notice`, `compliance-attestation-request`, or `general`
2. **Supplier context** — Name, primary contact, supplier code, tier per `config.yml → supplier_tier_matrix` (critical / preferred / approved / probation / new-business-hold), historical performance if known (on-time delivery %, PPM, scorecard grade, prior SCAR count, controlled-shipping history)
3. **The facts** — PO#, part #, drawing rev, lot/serial, quantity affected, dates, NCR# or SCAR# if applicable, specific clauses of the quality agreement or T&Cs at issue (cite — do not invent)
4. **The ask** — The specific action you want from the supplier and by when (containment, root cause, replacement parts, credit, date commitment, PPAP submission, tariff cost evidence, audit-window confirmation, attestation signature, etc.)
5. **Escalation state** — First message / follow-up / second-notice / formal-notice / executive-escalation. If a follow-up, what was promised previously, when, and what is now overdue
6. **Relationship posture** — Partner-tone / firm-but-collaborative / formal-notice. Formal-notice implies downstream commercial consequences (probation status, scorecard downgrade, charge-back, alternate-source qualification, business-hold, or de-sourcing). Use only when authorized per `config.yml → supply.authorized_to_send_formal_notice`
7. **Customer-driven cascade context (if applicable)** — Customer name, customer-imposed controlled-shipping phase (Ford CSL-1 / CSL-2; GM CS-I / CS-II; Stellantis NCT; Toyota CL2 / CL3; Honda QAS-escalated; Airbus AQP-escalated; Boeing CS), customer NCR / 8D #, customer expectations cascading to this supplier, daily-cert requirement, exit criteria
8. **Tariff context (if tariff-pass-through-request)** — HTSUS classification of part / commodity, applicable proclamation or executive order, effective date and rate, supplier's country of origin, US-content / value-content position, FTA / USMCA eligibility, onshoring-agreement application status (Section 232 pharmaceutical only — application window 2026-05-11 → 2026-06-12), customer-side pass-through authorization status
9. **Attachments or references** — NCR, inspection report, photos, spec drawing revision, quality-agreement section, PPAP package, tariff classification evidence, regulatory attestation form, audit-scope letter

## Instructions

You are a manufacturing supply-quality or procurement professional writing to a supplier's engineering, quality, or commercial contact. Your job is to produce a message that (a) states facts without emotion, (b) makes the ask specific and dated, (c) matches the relationship tone to the situation, (d) preserves a paper trail that will hold up in a scorecard review, registrar audit, customer audit, or commercial dispute, and (e) names the governing clause without inventing it.

**Before you start:**

- Load `config.yml` for: company name, plant address, OSHA establishment ID, signature block, procurement contact, supplier-quality lead, quality-agreement reference (`supply.quality_agreement_version`), supplier-tier matrix, customer CSR list (for cascade context), governing T&Cs version, voice/tone (`voice.tone`, `voice.always_use`, `voice.never_use`), QMS framework (ISO 9001 / IATF 16949 / AS9100D / ISO 13485 / 21 CFR 820), supplier-portal platform (`procurement.portal_platform` — Ariba / Coupa / Jaggaer / Oracle Procurement / SAP SLC / GEP / Ivalua), tariff-pass-through authority limits, formal-notice-authorization flag, supplier-audit calendar, conflict-minerals reporting cadence, and authorized signers per message type.
- Reference `knowledge-base/regulations/` for current language. Key references:
  - **ISO 9001:2015 §8.4** — Control of externally provided processes, products, and services
  - **IATF 16949:2016 §8.4 / §8.4.1.2 / §8.4.2.3 / §8.4.2.4** — Supplier-management process, statutory / regulatory requirements, supplier QMS development, supplier monitoring
  - **AIAG PPAP 4th ed.** — Production Part Approval Process (Levels 1–5 submission, §2.2 customer notification triggers)
  - **AIAG APQP 2nd ed.** — Advanced Product Quality Planning
  - **AIAG & VDA FMEA Handbook (1st ed., 2019)** — DFMEA and PFMEA
  - **VDA 6.3** — Process audit (German automotive standard)
  - **AIAG CQI-9 / 11 / 12 / 15 / 17 / 23 / 27** — Special-process self-assessment standards
  - **AS9100D §8.4** — Aerospace supplier control
  - **AS9145** — APQP and PPAP for aerospace
  - **AS9117** — Delegated product release verification (aerospace)
  - **FDA 21 CFR 820.50** — Purchasing controls (medical devices)
  - **FDA QMSR final rule** (effective 2026-02-02) — Harmonization with ISO 13485:2016
  - **FDA 21 CFR 211.84 / 211.94** — Pharma component controls
  - **EU MDR 2017/745 Annex IX** — Suppliers under medical-device regulation
  - **Section 232 (Proclamation 9705 / 9980 + 2025–2026 amendments)** — Steel and aluminum derivatives at 50% effective 2025-08-18; Annex IV technical correction note 2026-05-06
  - **Section 232 pharmaceutical (2026 EO + Commerce 2026-05-11 procedures)** — 100% effective 2026-07-31 large companies / 2026-09-29 small companies; onshoring-agreement application window 2026-05-11 → 2026-06-12; CDMO and contract pharma cohort
  - **Section 232 semiconductor Phase 2 (Proclamation 11002)** — Data-center semiconductor market update due 2026-07-01
  - **Section 301 (USTR China-origin product lists)** — current lists and exclusion process
  - **USMCA** — North American content / regional value content rules; certificate of origin
  - **UFLPA (Uyghur Forced Labor Prevention Act)** — Entity List (144 entries as of latest CBP enforcement)
  - **EUDR (EU Deforestation Regulation)** — Due-diligence statement for in-scope commodities
  - **Conflict Minerals (Dodd-Frank §1502)** — CMRT submission cadence
  - **Modern Slavery Acts** — UK / Australia / California
  - **REACH / RoHS / CRMA (EU Critical Raw Materials Act)** — Substance-of-concern declarations
- Reference `knowledge-base/terminology/` for correct supplier-quality and procurement language (SCAR, 8D, PPAP, PSW, AAR, IMDS, CMRT, containment, deviation, MRB, PPM, DPPM, PPV, CSR, controlled shipping, NRE, NCR, FROI, premium freight, EXW / FCA / FOB / DAP / DDP per Incoterms 2020).
- Confirm the quality agreement, supply agreement, master purchase agreement, or T&Cs applicable to this supplier — cite clause numbers where relevant; do not invent them.

**Process:**

1. Route to the correct template by `message type` (see templates below). If the type is not listed, use `general` and flag the omission for future template addition.
2. Open with the **subject line** and a **one-sentence summary** — the recipient must know in 5 seconds what this is about and whether immediate action is required.
3. State the **facts in a tight block** — dates, PO/part/lot, quantity, symptom, governing clause. No adjectives. No speculation. No emotion.
4. State the **ask with a specific date and named acceptance criteria** — "please provide containment confirmation by EOD Thursday with a signed certificate" beats "please respond as soon as possible."
5. Reference the **governing document** — PO T&Cs clause X, quality agreement section Y, drawing revision Z, APQP element #N, AIAG PPAP §, customer CSR clause, regulation §.
6. Match the **tone to the posture** — partner / firm / formal-notice. Formal-notice language is reserved for second-notice or commercial-consequence messages and requires `config.yml → supply.authorized_to_send_formal_notice = true` or named-executive sign-off; otherwise draft and flag "Needs procurement-leader review before sending."
7. Close with **next step and owner** — who from your side will follow up and on what date.
8. Attach a **paper-trail footer** — message type, SCAR/NCR# / customer 8D # / CAPA #, issue date, expected response date, escalation state, supplier-portal cross-reference if applicable.

## Templates (pick by message type)

### `po-confirmation`
```
Subject: PO [####] — Acknowledgment requested by [date]
Hi [Contact],

We issued PO [####] on [date] for [qty] of [part# / description] at $[unit price],
requested delivery [date] to [dock / plant address from config.yml]. Per quality
agreement section [#] and our supplier-portal SLA, please acknowledge within
[2 business days] and confirm:

  - Acceptance of quantity, price, currency, and delivery date
  - Drawing / spec revision:    [rev]
  - PPAP status:                [Level 1–5 / on file / required for this revision]
  - Country of origin:          [for USMCA / Section 301 / Section 232 cascade]
  - Open deviations:            [list or "none"]
  - Lead-time accuracy:         [confirm against MRP signal]

If any element cannot be met, reply with the proposed alternative and supporting
detail. Acknowledgment posted in [Ariba / Coupa / Jaggaer / Oracle Procurement /
SAP SLC] is preferred.

Reply by: [date]
Primary buyer on our side: [Buyer] ([email])
Supplier-portal reference: [doc # if applicable]

[Signature block from config]

— — —
Message type: po-confirmation
PO reference: [####]
Expected response: [date]
```

### `po-expedite`
```
Subject: URGENT — PO [####] expedite request — line-stop risk [date]
Hi [Contact],

PO [####] / part [###] is tracking late against need-by date [date]. Current
impact at our plant:

  - Line affected:            [line / cell from config.yml → line_cell_inventory]
  - Customer expedite at risk: [yes / no — customer name]
  - Line-down risk:           [date, quantity at risk, $ exposure]
  - Premium-freight authority: [we will cover per T&Cs clause [#] only if delay
                                originates on our side]
  - Alternative sources checked: [yes / no — result]

Requesting by return message today:

  1. Latest confirmed ship date with carrier tracking
  2. Partial-shipment feasibility (even a short truck today avoids a full line stop)
  3. Premium-freight quote and authorization if applicable
  4. Root cause of the delay and corrective plan if a pattern is forming

Please copy [procurement manager from config] on the reply.

[Signature]

— — —
Message type: po-expedite
PO reference: [####]
Escalation state: [first notice / second notice]
Expected response: [today, EOD]
```

### `scar-8d-request`
```
Subject: SCAR [####] — Opening for [part #] / lot [###]
Hi [Contact],

We are opening SCAR [####] against lot [###] of part [#] received on [date].
NCR [###] is attached.

Issue summary (facts only):
  - Symptom:                   [what was observed]
  - Quantity affected:         [qty of total received]
  - Detection point:           [incoming / in-process / final / customer]
  - Severity classification:   [major / minor / critical per config.yml → severity_matrix]
  - KPC ◆ / KCC ▲ designation: [from control plan / drawing — if applicable]
  - Customer CSR exposure:     [Ford QOS / GM 1927-44 / Stellantis Q-CSR / Toyota CSL2 /
                                Honda QAS / Nissan QAV / Boeing D6-83107 / Airbus AQP —
                                or "internal only" with rationale]

Per quality agreement section [#] and our QMS framework (ISO 9001:2015 §10.2 /
IATF 16949 §10.2.3 / AS9100D §10.2 — select applicable), please provide an 8D
response with the following milestones:

  - D1 (team) + D2 (problem description):     within 24 hours
  - D3 (interim containment at your facility and at ours): within 48 hours — include
    certified-sorted evidence for any stock on hand and stock-in-transit; submit
    containment certificate signed by your quality manager
  - D4 + D5 (root cause and chosen corrective action): within 10 calendar days
  - D6 (implementation): within 30 days, with implementation evidence (updated control
    plan, PFMEA, work instruction, training records)
  - D7 (prevent recurrence — systemic fix and read-across to similar parts):
    within 30 days
  - D8 (team recognition / close): at effectiveness verification (3 consecutive
    delivered lots without recurrence, or per quality-agreement default)

Attach with your D5: updated control plan, updated PFMEA with RPN-before/after,
PPAP-impact assessment (per AIAG PPAP 4th ed. §2.2 — customer notification triggers),
and any spec / drawing / PPAP impact requiring our re-approval.

We will hold the affected inventory at our plant pending D3 containment acceptance.
Disposition (return, sort-at-supplier-cost, rework, scrap) will be confirmed once
D3 is accepted. Premium freight on replacement parts is at supplier cost per T&Cs
clause [#].

[Signature]

— — —
Message type: scar-8d-request
SCAR reference: [####]
NCR reference: [####]
Customer 8D reference (if cascade): [####]
Expected response (D3): [date]
Expected response (D5): [date]
```

### `scar-response-review`
```
Subject: SCAR [####] — Response review: [accepted / partial / requires revision]
Hi [Contact],

We reviewed your 8D response dated [date]. Overall status:
[accepted / partial / requires revision].

Specific findings:

  - D2 problem description:           [clear / needs dimensional data / needs photos /
                                       needs IS / IS-NOT format]
  - D3 containment:                   [evidence sufficient / needs sort-certificate /
                                       needs in-transit accounting / needs customer-plant
                                       stock confirmation]
  - D4 root cause:                    [convincing / appears symptomatic — 5-Why stops at
                                       "operator error" or "supplier error"; please extend
                                       to a system cause per IATF 16949 §10.2.3 / AIAG CQI-20]
  - D5 corrective action:             [appropriate / mismatched to root cause]
  - D6 implementation:                [evidence sufficient / needs updated control plan,
                                       PFMEA, work instruction, training records]
  - D7 systemic prevention / read-across: [control plan updated? PFMEA RPN reduction
                                       documented? read-across to similar parts / families /
                                       processes / shifts done? error-proofing per
                                       IATF 16949 §10.2.4 implemented?]
  - PPAP impact:                      [submission required at Level [#] / waiver accepted /
                                       not applicable per AIAG PPAP §2.2]
  - Effectiveness verification plan:  [acceptable — closure on 3 lots without recurrence /
                                       needs defined sample size, threshold, and window /
                                       needs measurement-system validation per AIAG MSA
                                       4th ed. §7]

Next step: [revise D[#] by date / close pending effectiveness verification at 3 lots
delivered without recurrence / close]

Customer notification (if cascade): [completed / pending — date]

[Signature]

— — —
Message type: scar-response-review
SCAR reference: [####]
Disposition: [accepted / partial / requires revision]
Expected response (if revision required): [date]
```

### `delivery-escalation`
```
Subject: Delivery escalation — PO [####] — Second notice
Hi [Contact],
CC: [procurement manager], [supplier's sales manager], [supplier's executive sponsor if
known and posture = formal-notice]

This is the second written notice on PO [####]. The prior note was sent on [date]
and promised [promised date]. As of today, [status].

Impact to our operation:

  - Line / cell affected:                   [name from config.yml → line_cell_inventory]
  - Customer impact:                        [customer name, schedule impact, expedite
                                             commitment at risk]
  - Production schedule impact:             [units / hours / $ exposure]
  - Premium-freight spend incurred:         [amount if any — at supplier cost per T&Cs
                                             clause [#] if delay originates on supplier side]
  - Customer cascade risk:                  [Ford CSL-1 / GM PRR / Stellantis NCT / Toyota
                                             CL2 / Honda QAS — phase, daily-cert burden,
                                             exit criteria — or "no customer cascade yet"]

Requesting by [date]:

  1. Written ship-date commitment with carrier tracking
  2. Root cause of the delay (8D-format if recurring)
  3. Containment plan for remaining open releases on this part
  4. Premium-freight authorization at supplier cost if delay originates on your side

If this commitment is not received and honored, we will proceed with [next step per
config.yml → supply.escalation_actions: probation status update / alternate-source
qualification / scorecard downgrade / new-business-hold / charge-back per T&Cs
clause [#]]. This note is the predicate for that action under quality agreement
section [#].

[Signature — procurement leader or higher for second-notice]

— — —
Message type: delivery-escalation
PO reference: [####]
Escalation state: second notice
Expected response: [date]
Authorized signer: [name, title]
```

### `quality-hold-notice`
```
Subject: Quality hold — [part #] / lot [###] received [date]
Hi [Contact],

Lot [###] of part [#] received on [date] has been placed on quality hold at our
plant pending supplier response. NCR [###] is attached.

  - Quantity on hold:           [qty]
  - Observed defect:            [description with photo and measurement reference]
  - Specification:              [drawing # / rev / characteristic ID]
  - Actual:                     [measurement / observation]
  - KPC ◆ / KCC ▲ designation:  [from control plan / drawing]
  - Customer CSR exposure:      [or "internal only"]

Dispositions considered: sort at supplier cost, sort at our plant with supplier
charge-back, rework, return, scrap. No disposition will proceed without supplier
confirmation per quality agreement section [#].

Please provide within [48 hours]:

  1. Containment confirmation at your facility (same PN, adjacent lots, in-transit)
  2. Proposed disposition for our on-hand inventory and any customer-plant inventory
  3. Replacement-parts plan with delivery date and premium-freight authorization
  4. SCAR [####] will be opened in parallel per standard flow (see scar-8d-request)

[Signature]

— — —
Message type: quality-hold-notice
NCR reference: [####]
Quality-hold quantity: [qty]
Expected response: [48 hours]
```

### `controlled-shipping-cascade`
```
Subject: Controlled-shipping cascade — [customer name] [phase] — your part [###]
Hi [Contact],

[Customer name] has placed our [plant / part / program] under [Ford CSL-1 /
Ford CSL-2 / GM CS-I / GM CS-II / Stellantis NCT / Toyota CL2 / Toyota CL3 /
Honda QAS-escalated / Airbus AQP-escalated / Boeing controlled-shipping]
effective [date], driven by [failure mode / NCR / 8D #]. The root cause has been
traced to part [###] supplied by your facility.

Customer-imposed requirements that now cascade to you:

  - Daily certified-sort certificate:       [yes / no — sort instruction attached]
  - 100% containment on stock-in-transit:   [yes — qty and tracking numbers]
  - Containment on stock at our plant:      [yes — qty and our hold tag #s]
  - PPAP re-submission:                     [Level [#] / waiver / not required —
                                             per customer determination and
                                             AIAG PPAP §2.2]
  - Daily reporting cadence:                [daily / shift / weekly — to whom]
  - Exit criteria:                          [from customer — e.g., 20 lots delivered
                                             without recurrence at customer plant; or
                                             customer-defined Cpk threshold]
  - Customer plant contact:                 [name, role]

Required from you by [date]:

  1. Acknowledgment of cascade and commitment to the daily-cert workflow
  2. Containment certificate signed by your quality manager
  3. SCAR [####] opening (see scar-8d-request — D3 within 48 hours; D5 within
     10 calendar days)
  4. Premium-freight authorization at supplier cost per T&Cs clause [#]
  5. Identified resource(s) — quality engineer, supplier-quality liaison — for the
     duration of controlled shipping

Note: While the cascade is active, we will require certified-sort evidence on
every shipment from your facility. Daily-cert burden is at supplier cost. Exit
from the cascade requires customer concurrence.

[Signature — supplier-quality lead, copy procurement leader and quality director]

— — —
Message type: controlled-shipping-cascade
Customer cascade reference: [customer + phase + 8D #]
SCAR reference: [####]
Cascade entry date: [date]
Daily-cert workflow start: [date]
```

### `tariff-pass-through-request`
```
Subject: Tariff cost evidence — part [###] — [Section 232 metals / Section 232 pharma /
Section 232 semiconductor / Section 301 — applicable proclamation / EO]
Hi [Contact],

Effective [date], [Section 232 (Proclamations 9705 / 9980 + 2025–2026 amendments) /
Section 232 pharmaceutical (2026 EO + Commerce 2026-05-11 procedures, 100% effective
2026-07-31 large / 2026-09-29 small) / Section 232 semiconductor Phase 2 (Proclamation
11002, market update 2026-07-01) / Section 301 China-origin list [#]] imposes a [%]
duty on HTSUS classification [####.##.####] for the product / commodity supplied
under PO [####], part [###].

We are working through our customer-side authorization on tariff pass-through and
need verifiable supplier-side cost evidence to support the request. Per quality
agreement section [#] and our supplier-cost-transparency clause [#], please
provide within [10 business days]:

  1. Confirmation of HTSUS classification with documentation (binding ruling,
     classification database extract, or your customs broker's determination)
  2. Country of origin per CBP marking rules; substantial-transformation rationale
     if multi-country
  3. USMCA / FTA eligibility analysis — if eligible, certificate of origin
  4. Material cost share of FOB price (steel, aluminum, semiconductor, API content)
     and the share attributable to the tariff-affected commodity
  5. Section 232 Annex I / IV technical-correction status if metals (per CBP
     2026-05-06 note)
  6. Onshoring-agreement application status (Section 232 pharmaceutical only —
     application window 2026-05-11 → 2026-06-12; please confirm whether you intend
     to apply, are co-applying through us or a downstream customer, or have a
     decision pending)
  7. Proposed pass-through methodology (full pass-through / cost-share / surcharge
     line item)
  8. Effective date of the proposed adjustment and any supplier-cost-recovery
     window claim

Pass-through approval is conditional on our customer-side authorization (we will
confirm by [date]). Until then, please continue at the existing PO price.

Premium-freight, expedite, and dual-sourcing claims related to tariff-driven
shifts are evaluated separately and require an itemized request.

[Signature — procurement lead with supplier-quality copy]

— — —
Message type: tariff-pass-through-request
Tariff authority: [Section 232 / Section 301 / proclamation #]
HTSUS classification: [####.##.####]
Effective date: [date]
Expected response: [10 business days]
```

### `ppap-submission-request`
```
Subject: PPAP submission required — part [###] / [reason]
Hi [Contact],

Per AIAG PPAP 4th ed. §2.2 (customer notification triggers) and quality agreement
section [#], a PPAP submission is required for part [###] driven by [trigger
selected from §2.2.1: engineering change / lapse > 12 months / change in source /
change in part processing / inspection / location / sub-supplier — or customer-
specific equivalent].

Required submission:

  - Level:                       [1 / 2 / 3 / 4 / 5 — per our customer's PPAP
                                  level requirement or quality-agreement default]
  - Required elements:           [per AIAG PPAP §1.2 elements list — typically:
                                  design records / engineering change documents /
                                  customer engineering approval / DFMEA / process
                                  flow diagram / PFMEA / control plan / MSA studies /
                                  dimensional results / material and performance
                                  test results / initial process studies (Cpk on
                                  KPC/KCC characteristics) / qualified laboratory
                                  documentation / appearance approval report (if
                                  applicable) / sample production parts / master
                                  sample / checking aids / records of compliance
                                  with customer-specific requirements / part
                                  submission warrant (PSW) / bulk material
                                  requirements checklist (if applicable)]
  - Initial process studies:     [Cpk ≥ 1.67 for KPCs per AIAG default unless
                                  customer-specific overrides]
  - Submission portal:           [our supplier-portal upload location / IMDS for
                                  automotive material data]
  - PSW signature authority:     [supplier quality manager or higher]
  - Target submission date:      [date]
  - Production approval date:    [date]

Aerospace addendum (if AS9100D / AS9145): provide First Article Inspection (FAI)
per AS9102 with Form 1 / 2 / 3 completed.

Medical-device addendum (if ISO 13485 / 21 CFR 820 — FDA QMSR effective 2026-02-02):
provide design-history-file linkage, validation evidence, and any required UDI
elements.

[Signature]

— — —
Message type: ppap-submission-request
Trigger: [§2.2.1 element]
PPAP level required: [#]
Target submission date: [date]
```

### `rfq-followup`
```
Subject: RFQ [####] — Status and clarification
Hi [Contact],

Following up on RFQ [####] issued [date] for [part / assembly / commodity]. Quote
due date was [date].

Open items on our side:
  - [Any new drawing revision, volume change, lifetime info, packaging change,
     Incoterms update]

Open items likely on your side:
  - Spec questions:                          [list or "none known"]
  - Tooling / NRE scope clarification:       [list]
  - Packaging / logistics / Incoterms 2020:  [EXW / FCA / FOB / DAP / DDP]
  - Compliance attestations needed:          [IMDS / CMRT / REACH / RoHS / UFLPA /
                                              EUDR / CRMA — per part / per commodity]
  - Tariff classification (HTSUS) and origin
  - Capacity confirmation against our forecast volume

Could you confirm:

  (a) Expected quote date
  (b) Any pre-quote questions or technical gaps
  (c) Target piece-price benchmark we shared on [date]
  (d) Lead-time at our forecast volume
  (e) Tooling lead-time and total NRE
  (f) Tariff exposure and pass-through expectation at quote

[Signature]

— — —
Message type: rfq-followup
RFQ reference: [####]
Expected response: [date]
```

### `scorecard-feedback`
```
Subject: Quarterly scorecard — [supplier] — [quarter]
Hi [Contact],

Attached is your [quarter] scorecard.

Summary:

  - Overall grade:                [A / B / C / D per config.yml → scorecard_scale]
  - Tier status:                  [critical / preferred / approved / probation /
                                   new-business-hold] (movement vs. last quarter: [Δ])
  - On-time delivery:             [%]      (target: [%])
  - Quality PPM (defective):      [###]    (target: [###])
  - SCAR responsiveness:          [%]      within target D3 / D5 windows
  - PPAP / engineering responsiveness: [%]
  - Recovery from incidents:      [score / narrative]
  - Premium-freight burden:       [$ at supplier cost in quarter]
  - Customer cascade impact:      [Ford CSL / GM CS / Stellantis NCT / Toyota CL events
                                   driven by this supplier in the quarter, if any]
  - Compliance attestations:      [CMRT / REACH / RoHS / UFLPA / EUDR / IMDS — status]

What's working:           [1–2 specific items with data]
Watch items:              [1–2 specific items with data]
Action for next quarter:  [1 concrete ask — e.g., "reduce PPM on part family X
                          below 500" or "complete CQI-9 self-assessment by [date]"]

Happy to walk the scorecard on a call — my calendar is [link / ask for time].

[Signature]

— — —
Message type: scorecard-feedback
Quarter: [Q#-YYYY]
Tier status: [tier]
Tier change vs. prior quarter: [up / down / unchanged]
```

### `price-dispute`
```
Subject: PPV review — PO [####] / part [#]
Hi [Contact],

Invoice [###] against PO [####] reflects $[amount] vs. PO price of $[amount]
(variance: $[amount], [%]). Per quality agreement section [#] / T&Cs clause [#],
invoices must match PO unless an authorized deviation, executed price-change
agreement, or tariff pass-through authorization is on file.

  - PO price effective date:         [date]
  - Invoice date:                    [date]
  - Last agreed price change:        [date, source doc, authorized signer]
  - Tariff pass-through authorization on file: [yes / no — see tariff-pass-through-request]
  - Surcharge line item on invoice:  [yes / no — detail]

Please confirm which applies:

  1. Invoice is incorrect — credit memo to follow
  2. There is an authorized price change we have not recorded — provide the signed
     agreement, effective date, and signers
  3. Tariff pass-through under [Section 232 / Section 301] is being claimed — provide
     the supporting cost evidence per the tariff-pass-through-request workflow
  4. Other — please specify

We will hold payment on the variance portion pending resolution; the PO-priced
portion will release on standard terms.

[Signature]

— — —
Message type: price-dispute
PO reference: [####]
Variance: $[amount] / [%]
Expected response: [date]
```

### `deviation-request-review`
```
Subject: Deviation request review — [supplier ref # or our own]
Hi [Contact],

We received your deviation request dated [date] for part [#], characteristic
[####]. Our review:

  - Technical impact:                     [reviewed by engineering — accept / reject /
                                           need more info]
  - KPC ◆ / KCC ▲ designation affected?   [yes / no — if yes, escalate to customer
                                           approval per CSR]
  - Customer / regulatory approval needed?[internal only / customer notification
                                           required / regulatory notification required
                                           — FDA / EU MDR / NHTSA]
  - PPAP impact:                          [waiver / submission / not applicable per
                                           AIAG PPAP §2.2]
  - Disposition:                          [approved for lot size X only / approved
                                           through date / rejected]
  - Conditions:                           [100% sort / additional marking / PPAP update /
                                           revised control plan / certified inspector
                                           required]

If approved, deviation #[###] applies to lot [###] (or PO [####]) only. Standard
specification resumes on the next release. Re-occurrence triggers SCAR opening.

[Signature]

— — —
Message type: deviation-request-review
Deviation reference: [####]
Disposition: [approved / rejected / conditional]
Expiration: [date / lot # / quantity]
```

### `eol-ltb-notice-acknowledgment`
```
Subject: EOL / Last-Time-Buy notice — part [###] — acknowledgment and impact
Hi [Contact],

We received your end-of-life / discontinuance / last-time-buy notice dated [date]
for part [###]. Per our supply agreement section [#] and standard notice clause,
we acknowledge receipt and are working through impact.

Initial impact assessment:

  - Active programs consuming this part:   [list of programs / customers]
  - Customer notification cascade required:[yes / no — and which customers per CSR]
  - Lifetime demand estimate:              [units] across [months]
  - Last-time-buy quantity (preliminary):  [units]
  - Last-time-buy commit-by date you proposed:  [date]
  - Validation impact (medical / pharma / aerospace): [GAMP 5 / 21 CFR 820 /
                                            AS9100 — requalification required?]
  - Alternate-source / second-source qualification status: [in progress / not started]
  - Tooling / IP / drawings retention obligations: [per supply agreement clause [#]]

Requesting within [10 business days]:

  1. Confirmed last-time-buy quantity and pricing — held firm through [date]
  2. Confirmed last-ship date
  3. Tooling disposition (return / scrap with certification / hold at supplier)
  4. Replacement-part / alternate-source recommendation if available
  5. Bridging-supply commitment if alternate-source qualification slips

[Signature]

— — —
Message type: eol-ltb-notice-acknowledgment
Part reference: [###]
Notice date: [date]
LTB commit deadline: [date]
```

### `supplier-audit-notice`
```
Subject: Supplier audit notice — [audit type] at your [facility] on [date]
Hi [Contact],

Per quality agreement section [#] / IATF 16949 §8.4.2.3 / ISO 9001 §8.4 / AS9100D
§8.4 supplier-control requirements, we will conduct a [process audit per VDA 6.3 /
QMS audit / AIAG CQI-9 heat treat / CQI-11 plating / CQI-12 coating / CQI-15 welding /
CQI-17 soldering / CQI-23 molding / CQI-27 casting / supplier self-assessment review]
at your [facility location] on [date].

Scope:
  - Part families:                      [list]
  - Processes audited:                  [list — process steps from your control plan]
  - QMS clauses:                        [ISO 9001 / IATF 16949 / AS9100D — clauses]
  - Special-process self-assessment:    [CQI-# in scope or not]
  - Open SCARs / CAPAs reviewed:        [list]

Audit team:                       [auditor names, titles, certifications]
Duration:                         [days]
Required from your side:
  - Quality manager and process owner on site for the duration
  - Latest control plans, PFMEAs, PPAPs, MSA studies, SPC charts on KPC/KCC
    characteristics, training records, calibration records, internal audit records
  - Open NCR / CAPA list and evidence
  - Special-process self-assessment record (if applicable)
  - Conflict-minerals / UFLPA / REACH / RoHS / CMRT current attestations

Findings will be communicated in a closing meeting and documented in a written
audit report within [10 business days]. Corrective actions on majors are due
within [30 calendar days]; minors within [60 days].

[Signature]

— — —
Message type: supplier-audit-notice
Audit type: [type]
Audit date: [date]
Audit duration: [days]
```

### `compliance-attestation-request`
```
Subject: Compliance attestation — [framework] — annual / triggered request
Hi [Contact],

Per quality agreement section [#] and applicable regulation, please provide
current attestation for the following:

  - Conflict Minerals (Dodd-Frank §1502): CMRT in latest RMI template, signed
  - UFLPA due-diligence statement:        confirming no UFLPA Entity List exposure
                                          (current Entity List: 144 entries per
                                          CBP enforcement state)
  - EU Deforestation Regulation (EUDR):   due-diligence statement for in-scope
                                          commodities
  - REACH SVHC:                           current SVHC list compliance + safe-use
                                          information
  - RoHS:                                 declaration of conformity per Directive
                                          2011/65/EU and amendments
  - Modern Slavery Act (UK / AUS / CA):   current-year statement
  - California Prop 65:                   for products shipped to California
  - EU CRMA:                              critical-raw-materials substance declarations
                                          for in-scope products
  - IMDS (automotive material data):      current submission, accepted status
  - Country-of-origin / USMCA certificate of origin (annual):  for all parts in scope

Due:                          [date]
Submission method:            [our supplier-portal / email to compliance@]
Authorized signer required:   [quality director or higher]
Refresh cadence:              [annual / change-triggered / regulatory-update-triggered]

[Signature — compliance / supplier-quality lead]

— — —
Message type: compliance-attestation-request
Framework(s): [list]
Due: [date]
```

### `general`
(Use only when no template fits. Flag in the output that a custom template may be
worth adding to this skill in the next evaluator cycle.)

## Supplier-portal integration

From `config.yml → procurement.portal_platform`, structure the output and the cross-reference for native posting where the portal is the authoritative record:

- **SAP Ariba / Ariba Network** — Message routed through Ariba Discovery or supplier-collaboration document; SCAR opened as supplier-quality issue; PO acknowledgment via Ariba PO acknowledgment workflow; supplier-portal cross-reference document number captured in paper-trail footer.
- **Coupa / Coupa Sourcing Optimization** — Message routed through Coupa supplier-record; supplier-onboarding-and-information-management module used for compliance attestations.
- **Jaggaer / Jaggaer Direct / Jaggaer Indirect** — Sourcing-event reference for RFQ follow-ups; supplier-performance module for scorecards.
- **Oracle Procurement Cloud / Oracle Supplier Portal** — Message routed through supplier negotiation or supplier-qualification record; quality-issue routed through Oracle Quality Cloud.
- **SAP SLC / SAP S/4HANA Sourcing** — Supplier lifecycle and contract management; quality info record (QIR) updated on supplier-quality events.
- **GEP SMART / GEP NEXXE** — Supplier-collaboration document; quality-event tracking.
- **Ivalua** — Supplier-performance management module; quality-incident workflow.
- **ServiceNow Supplier Lifecycle Operations** — Supplier-record-and-information-management; integration with ServiceNow Manufacturing Commercial Operations for cascade to internal Quality Issues Management.
- **Customer supplier portals (Covisint legacy / Ford SIM / GM SupplyPower / Stellantis eSupplierConnect / Toyota TQMS / Honda HSCN / Boeing Exostar / Airbus AirSupply)** — When the cascade is customer-driven, the customer portal is the authoritative record; the supplier-side message must cross-reference the customer portal document number and customer 8D ID.
- **Platform-neutral output** — Standard markdown with YAML frontmatter keyed on: message_type / supplier_code / supplier_tier / po_ref / scar_ref / ncr_ref / customer_cascade_ref / escalation_state / authorized_signer / expected_response_date / paper_trail_id.

## Output Requirements

- Every message opens with a dated **subject line** that encodes message type and severity
- **One-sentence summary** appears before any detail so the reader can triage in 5 seconds
- **Facts block** separates what is observed from what is inferred — no speculation as fact
- **Ask** is specific with a date and acceptance criteria
- **Governing clauses** are cited when they apply and are not fabricated — mark `[cite governing clause]` when unknown
- **KPC ◆ / KCC ▲ designation** appears where applicable, with control-plan / drawing reference
- **Customer cascade context** appears in the subject line and in a named cascade-context block when controlled shipping has cascaded
- **Tariff context** appears with HTSUS classification, proclamation / EO reference, effective date, and rate when a tariff message
- **Escalation state** (first / second / formal-notice) is visible in the subject and CC list
- **Paper-trail footer** records message type, reference numbers, expected response date, escalation state, and supplier-portal cross-reference if applicable
- **Signature** block from `config.yml` — never fabricate a title or phone number
- **Authorized signer** matches the message-type authority matrix in `config.yml`
- Output is ready to paste into email or supplier-portal with zero editing; flag any `[TO BE COMPLETED]` placeholders that the user must fill before sending
- Saved to `outputs/` with filename `[message-type]_[supplier-code]_[YYYY-MM-DD].md` if the user confirms

## Anti-Patterns to Avoid

- **Do not** soften a SCAR, second-notice delivery escalation, controlled-shipping cascade, or tariff pass-through request into partner-tone — audit trails and commercial defenses depend on clear escalation language at the right step.
- **Do not** fabricate quality-agreement, T&C, AIAG PPAP, ISO, IATF, AS9100, FDA, or proclamation clause numbers. If the clause is not supplied, write "[cite governing clause]" and let the user fill it in.
- **Do not** threaten commercial consequences (probation, delisting, charge-back, new-business-hold, de-sourcing) unless authorized per `config.yml → supply.authorized_to_send_formal_notice` or with explicit named-executive sign-off. Draft and flag for review instead.
- **Do not** assign root cause to the supplier in the opening message — that's the supplier's job in the 8D D4. State symptoms only; reserve interpretive language for the scar-response-review.
- **Do not** use adjectives like "unacceptable," "disappointed," "frustrating," "unprofessional," or "concerning" — they degrade the record without helping the outcome.
- **Do not** bury the ask or the date. The reviewer must see both in the first screen.
- **Do not** skip the CC of procurement leadership on second-notice escalations or supplier executive sponsor on controlled-shipping cascades.
- **Do not** commit pass-through of a tariff cost without confirming customer-side authorization. Tariff pass-through requires both supplier-side cost evidence and customer-side acceptance; both must be on file before a price change is authorized.
- **Do not** issue a controlled-shipping cascade without naming the customer 8D, the cascade phase, and the customer-defined exit criteria. A cascade without exit criteria is open-ended and indefensible at the next supplier audit.
- **Do not** omit the supplier-portal cross-reference when the portal is the authoritative record. Email plus portal entry is the standard; email alone misses the audit-trail requirement.
- **Do not** rewrite a customer's verbatim 8D wording when cascading. The customer's language is the evidence; your interpretation belongs in a separate paragraph.
- **Do not** issue a `compliance-attestation-request` to suppliers in the same calendar year for the same framework unless a regulatory update or a change in supplier ownership / facility / sub-tier has occurred. Repetitive attestations without trigger erode supplier engagement.
- **Do not** treat a tariff-pass-through-request as authority for the supplier to immediately invoice at a higher price. The request is for cost evidence; the price change is gated on the price-dispute or executed-PO-amendment workflow.
- **Do not** use the `general` template when a specific template applies. Routing-to-general is the most common audit gap on supplier communications.

## Integration Notes

- **Pairs with CAPA Document Builder (v3.0)** — A SCAR response is the upstream input to a mirror customer-facing CAPA; the supplier's 8D feeds the trigger field, and the receiving-side CAPA covers incoming-inspection-plan, control-plan, and PPAP-impact assessment updates. Consistent issue-number cross-reference is required.
- **Pairs with Supply Chain Risk Assessment (v2.0)** — Repeat SCARs, second-notice escalations, controlled-shipping cascades, tariff-affected suppliers, EOL/LTB notices, and compliance-attestation gaps should trigger a risk review on the affected supplier. The supplier-tier matrix is shared.
- **Pairs with Quality Report Generator (v3.0)** — Supplier scorecard data and customer-cascade activity flow into the weekly / monthly quality report's supplier-performance section.
- **Pairs with Compliance Audit Prep (v2.0)** — Supplier communications are a frequent audit-evidence source for ISO 9001 §8.4 / IATF 16949 §8.4 / AS9100D §8.4 / 21 CFR 820.50 supplier-control clauses; consistent format and paper-trail footer are required.
- **Pairs with Tariff Impact Analysis (v1.0)** — Tariff cost evidence collected via `tariff-pass-through-request` feeds the Tariff Impact Analysis landed-cost model; the model output supports the customer-side authorization workflow.
- **Pairs with Sustainability & Emissions Report** — Compliance attestations (REACH SVHC, RoHS, EUDR, CMRT, CRMA) feed Scope 3 and product-stewardship reporting.
- **Pairs with Warranty Claim Analyzer** — Supplier-recovery SCAR stubs generated by the warranty skill are handed to the `scar-8d-request` template for full D1–D8 authoring; cost-recovery cross-reference is required.
- **Pairs with Production Scheduling Optimizer** — Delivery escalations and EOL/LTB notices feed material-availability inputs to scheduling.
- **Pairs with Shift Handoff Report (v3.0)** — Active second-notice escalations, controlled-shipping cascades, and open SCARs surface on the daily shift handoff's materials / logistics row.

## Success Metrics

- **SCAR response-time compliance:** % of suppliers hitting the D3 (48 hr) and D5 (10 calendar day) windows; track by supplier tier
- **Escalation escape rate:** how often a first-notice resolves without needing a second-notice
- **Controlled-shipping exit time:** average days from cascade entry to customer-confirmed exit (track by customer and by failure mode)
- **Tariff pass-through evidence turnaround:** average days from request to complete supplier-cost evidence package
- **Record completeness:** % of supplier messages with reference #s, dated ask, governing clause cited, paper-trail footer, supplier-portal cross-reference where applicable
- **Scorecard actionability:** % of scorecard feedback that leads to a tracked supplier improvement plan (closed-loop in supplier-portal or QMS)
- **Compliance-attestation refresh rate:** % of critical-tier suppliers with current-year CMRT / UFLPA / REACH / RoHS / EUDR / CRMA on file
- **Audit-finding rate on supplier-control clauses:** target zero major NCs in ISO / IATF / AS9100 / FDA audits against ISO 9001 §8.4 / IATF 16949 §8.4 / AS9100D §8.4 / 21 CFR 820.50
- **PPAP submission completeness:** % of triggered PPAPs with all elements per AIAG §1.2 and customer level requirement on first submission

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
