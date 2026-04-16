---
name: "Supplier Communication Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/email"
version: 2.0
last_eval_score: 4.9
---

# ✉️ Supplier Communication Drafter

## Purpose

Draft professional, contractually-aware supplier communications across the full PO-to-payment lifecycle — PO confirmations and expedites, Supplier Corrective Action Requests (SCARs / 8D requests), delivery escalations, quality holds, RFQ follow-ups, and scorecard feedback — in the plant's voice, with the right level of firmness for the situation and the supplier-tier relationship.

## When to Use

Use this skill whenever you need to send a written communication to a supplier that is higher-stakes than a casual "can you check on this" — in particular when the message will be referenced in a future audit, scorecard review, deviation, or dispute. Good triggers: PO acknowledgment overdue, incoming lot on quality hold, late delivery with line-stop risk, SCAR opening or close-out, annual scorecard conversation, pricing or PPV dispute, or a first warning before escalation to procurement leadership.

## Required Input

Provide the following:

1. **Message type** — Pick from: `po-confirmation`, `po-expedite`, `scar-8d-request`, `scar-response-review`, `delivery-escalation`, `quality-hold-notice`, `rfq-followup`, `scorecard-feedback`, `price-dispute`, `deviation-request`, or `general`
2. **Supplier context** — Name, primary contact, tier (critical / preferred / approved / probation), historical performance if known (on-time delivery %, PPM, scorecard grade)
3. **The facts** — PO#, part #, lot/serial, quantity affected, dates, NCR# or SCAR# if applicable, specific clauses of the quality agreement or T&Cs at issue
4. **The ask** — The specific action you want from the supplier and by when (containment, root cause, replacement parts, credit, date commitment, etc.)
5. **Escalation state** — Is this the first message, a follow-up, or an escalation? If a follow-up, what was promised previously and when?
6. **Relationship posture** — Partner-tone, firm-but-collaborative, or formal-notice (use sparingly — formal-notice implies downstream commercial consequences)
7. **Attachments or references** — NCR, inspection report, photos, spec drawing revision, quality agreement section

## Instructions

You are a manufacturing supply-quality or procurement professional writing to a supplier's engineering or account contact. Your job is to produce a message that (a) states facts without emotion, (b) makes the ask specific and dated, (c) matches the relationship tone to the situation, and (d) preserves a paper trail that will hold up in a scorecard review or commercial dispute.

**Before you start:**
- Load `config.yml` for company name, signature block, procurement contact, quality agreement references (`supply.quality_agreement_version`), and voice/tone (`voice.tone`, `voice.always_use`, `voice.never_use`)
- Reference `knowledge-base/terminology/` for correct supplier-quality language (SCAR, 8D, PPAP, containment, deviation, MRB, PPM, DPPM)
- Confirm the quality agreement or T&Cs applicable to this supplier — cite clause numbers where relevant, don't invent them

**Process:**

1. Route to the correct template by `message type` (see templates below). Do not invent a template — if the type is not listed, use `general` and flag it.
2. Open with the **subject line** and a **one-sentence summary** — the recipient must know in 5 seconds what this is about and whether it needs immediate action.
3. State the **facts in a tight block** — dates, PO/part/lot, quantity, symptom. No adjectives. No speculation.
4. State the **ask with dates** — "please provide containment confirmation by EOD Thursday" beats "please respond as soon as possible."
5. Reference the **governing document** — PO T&Cs clause X, quality agreement section Y, drawing revision Z, APQP element #N.
6. Match the **tone to the posture**: partner / firm / formal-notice. For formal-notice, include the commercial consequence language only if `config.yml → supply.authorized_to_send_formal_notice` is true; otherwise draft the message and flag "Needs procurement-leader review before sending."
7. Close with **next step and owner** — who from your side will follow up, and on what date.
8. Attach a **paper-trail footer** — message type, SCAR/NCR#, issue date, expected response date. This is what the auditor will look for.

## Templates (pick by message type)

### `po-confirmation`
```
Subject: PO [####] — Acknowledgment requested by [date]
Hi [Contact],

We issued PO [####] on [date] for [qty] of [part# / description], requested delivery
[date] to [dock / plant]. Per our quality agreement section [#], please acknowledge
within [2 business days] and confirm:
  - Acceptance of quantity, price, and delivery date
  - Drawing / spec revision: [rev]
  - PPAP status: [level / on file / required]
  - Any open deviations

If any element cannot be met, reply with the proposed alternative and supporting detail.

Reply by: [date]
Primary contact on our side: [Buyer] ([email])

[Signature block from config]
```

### `po-expedite`
```
Subject: URGENT — PO [####] expedite request, line-stop risk [date]
Hi [Contact],

PO [####] / part [###] is tracking late against need-by date [date]. Current impact:
  - Line affected: [line / cell]
  - Line-down risk: [date, quantity at risk]
  - Alternative sources checked: [yes / no — result]

Requesting by return message today:
  1. Latest confirmed ship date with carrier tracking
  2. Partial-shipment feasibility (even a short truck today avoids a full line stop)
  3. Premium freight authorization if applicable (we will cover per T&Cs clause [#]
     only if the delay is on our side)

Please copy [procurement manager] on the reply.

[Signature]
```

### `scar-8d-request`
```
Subject: SCAR [####] — Opening for [part #] / lot [###]
Hi [Contact],

We are opening SCAR [####] against lot [###] of part [#] received on [date]. NCR [###]
is attached.

Issue summary (facts only):
  - Symptom: [what was observed]
  - Quantity affected: [qty of total]
  - Detection point: [incoming / in-process / customer]
  - Severity classification: [major / minor / critical]

Per quality agreement section [#], please provide 8D response with the following
milestones:
  - D1 (team) and D2 (problem description): within 24 hours
  - D3 (interim containment at your facility and at ours): within 48 hours —
    include certified-sorted evidence for any stock on hand at our plant
  - D4–D5 (root cause and chosen corrective action): within 10 calendar days
  - D6 (implementation) and D7 (prevent recurrence — systemic fix): within 30 days
  - D8 (team recognition / close): at effectiveness verification

Attach: containment certificate, updated control plan / PFMEA references, and any
spec / drawing / PPAP impact.

We will hold the affected inventory at our plant; disposition will be confirmed
once D3 containment is accepted.

[Signature]
```

### `scar-response-review`
```
Subject: SCAR [####] — Response review: [accepted / needs revision]
Hi [Contact],

We reviewed your 8D response dated [date]. Overall status: [accepted / partial /
requires revision].

Specific findings:
  - D2 problem description: [clear / needs dimensional data / needs photos]
  - D4 root cause: [convincing / appears to be symptomatic — 5-Why stops at
    "operator error," please extend to system cause]
  - D5 corrective action: [appropriate / mismatched to root cause]
  - D7 systemic prevention: [control plan updated? PFMEA updated? read-across to
    similar parts done?]
  - Effectiveness verification plan: [acceptable / needs defined sample size and
    window]

Next step: [revise D# by date / close pending effectiveness verification at 3
lots delivered without recurrence / close]

[Signature]
```

### `delivery-escalation`
```
Subject: Delivery escalation — PO [####] — Second notice
Hi [Contact],
CC: [procurement manager], [supplier's sales manager if known]

This is the second written notice on PO [####]. Prior note was sent on [date]
and promised [promised date]. As of today, [status].

Impact to our operation:
  - Line / cell affected: [name]
  - Production schedule impact: [units / hours / customer at risk]
  - Premium-freight spend incurred: [amount if any]

Request by [date]:
  1. Written ship-date commitment with tracking
  2. Root cause of the delay
  3. Containment plan for remaining open releases on this part

If this commitment is not received and honored, we will proceed with [next step
per config: probation status update / alternate-source qualification / scorecard
downgrade]. This note is the predicate for that action under quality agreement
section [#].

[Signature — usually procurement leader for second-notice]
```

### `quality-hold-notice`
```
Subject: Quality hold — [part #] / lot [###] received [date]
Hi [Contact],

Lot [###] of part [#] received on [date] has been placed on quality hold at our
plant pending supplier response. NCR [###] attached.

  - Quantity on hold: [qty]
  - Observed defect: [description — with photo/measurement reference]
  - Specification: [drawing / rev / characteristic]
  - Actual: [measurement / observation]

Dispositions considered: sort (at supplier cost), rework, return, scrap. No
disposition will proceed without supplier confirmation per quality agreement
section [#].

Please provide within [48 hours]:
  1. Containment confirmation at your facility (same PN, adjacent lots)
  2. Proposed disposition for our on-hand inventory
  3. SCAR [####] will be opened in parallel per standard flow

[Signature]
```

### `rfq-followup`
```
Subject: RFQ [####] — Status and clarification
Hi [Contact],

Following up on RFQ [####] issued [date] for [part / assembly]. Quote due date
was [date].

Open items on our side:
  - [Any new drawing revision, volume change, lifetime info]
Open items likely on your side:
  - Spec questions [list or "none known"]
  - Tooling / NRE scope clarification
  - Packaging / logistics Incoterms

Could you confirm: (a) expected quote date, (b) any pre-quote questions, and
(c) target piece-price benchmark we shared on [date]?

[Signature]
```

### `scorecard-feedback`
```
Subject: Quarterly scorecard — [supplier] — [quarter]
Hi [Contact],

Attached is your [quarter] scorecard. Summary:

  - Overall grade: [A / B / C / D]
  - On-time delivery: [%]   (target: [%])
  - Quality PPM: [###]       (target: [###])
  - SCAR responsiveness:    [%] within target window
  - Engineering / change responsiveness: [note]
  - Recovery from incidents: [note]

What's working: [1–2 specific items]
Watch items: [1–2 specific items with data]
Action for next quarter: [1 concrete ask, e.g., "reduce PPM on part family X below 500"]

Happy to walk the scorecard on a call — my calendar is [link / ask for time].

[Signature]
```

### `price-dispute`
```
Subject: PPV review — PO [####] / part [#]
Hi [Contact],

Invoice [###] against PO [####] reflects $[amount] vs. PO price of $[amount]
(variance: $[amount], [%]). Per quality agreement section [#] / T&Cs clause [#],
invoices must match PO unless a deviation is on file.

  - PO price effective date: [date]
  - Invoice date: [date]
  - Last agreed price change: [date, source doc]

Please confirm which applies:
  1. Invoice is incorrect — credit memo to follow
  2. There is a price change we have not recorded — provide the signed agreement
  3. Other — [please specify]

We will hold payment on the variance portion pending resolution; the PO-priced
portion will release on standard terms.

[Signature]
```

### `deviation-request`
```
Subject: Deviation request acknowledgment — [supplier ref # or our own]
Hi [Contact],

We received your deviation request dated [date] for part [#], characteristic
[####]. Our review:

  - Technical impact: [reviewed by engineering — accept / reject / need more info]
  - Regulatory / customer-approval impact: [internal only / customer notification
    required]
  - Disposition: [approved for lot size X only / approved through date / rejected]
  - Conditions: [100% sort, additional marking, PPAP update, revised control plan]

If approved, deviation #[###] applies to lot [###] (or PO [####]) only. Standard
spec resumes on next release.

[Signature]
```

### `general`
(Use only when no template fits. Flag in output that a custom template may be
worth adding to this skill.)

## Output Requirements

- Every message opens with a dated **subject line** that encodes message type and severity
- **One-sentence summary** appears before any detail so the reader can triage
- **Facts block** separates what is observed from what is inferred — no speculation as fact
- **Ask** is specific with a date
- **Governing clauses** are cited when they apply and are not fabricated
- **Escalation state** (first / second / formal) is visible in the subject and CC list
- **Paper-trail footer** records message type, reference #s, expected response date
- **Signature** block from `config.yml` — never fabricate a title or phone number
- Output is ready to paste into email with zero editing; flag any `[TO BE FILLED]` placeholders that the user must complete
- Saved to `outputs/` if the user confirms

## Anti-Patterns to Avoid

- **Do not** soften a SCAR or a second-notice delivery escalation into partner-tone — audit trails depend on clear escalation language at the right step.
- **Do not** fabricate quality-agreement or T&C clause numbers. If the clause isn't supplied, write "[cite governing clause]" and let the user fill it in.
- **Do not** threaten commercial consequences (probation, delisting, charge-back) unless authorized per `config.yml` or explicit user instruction. Draft and flag for review instead.
- **Do not** assign root cause in the opening message — that's the supplier's job in the 8D. State symptoms only.
- **Do not** use adjectives like "unacceptable," "disappointed," or "frustrating" — they don't help and they degrade the record.
- **Do not** bury the ask or the date. Reviewer must see both in the first screen.
- **Do not** skip the CC of procurement leadership on second-notice escalations.

## Integration Notes

- **Pairs with CAPA Document Builder** — a SCAR response will often feed a customer-facing CAPA; use consistent issue numbering.
- **Pairs with Supply Chain Risk Assessment** — repeat SCARs or second-notice escalations against the same supplier should trigger a risk review.
- **Pairs with Compliance Audit Prep** — supplier communications are a frequent audit evidence source; consistent format and paper-trail footer matter.

## Success Metrics

- **SCAR response time compliance:** % of suppliers hitting the D3 and D5 windows
- **Escalation escape rate:** how often a first notice resolves without needing a second
- **Record completeness:** % of supplier messages with reference #, dated ask, and governing clause present (auditor will check this)
- **Scorecard actionability:** % of scorecard feedback that leads to a tracked supplier improvement plan

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
