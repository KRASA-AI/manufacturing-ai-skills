---
name: "Email Drafter"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/email"
version: 3.0
last_eval_score: 8.8
---

# Email Drafter

## Purpose

Draft a professional manufacturing email that uses the right industry vocabulary, separates facts from asks, names a clear next step with an owner and a date, and never fabricates an identifier (PO, NCMR, SCAR, RMA, lot number, drawing revision, certification clause). The output is an email that reads as if it were written by a quality manager, sales engineer, or operations leader inside an ISO-registered shop — not a generic business-communication template with manufacturing words sprinkled in.

The skill is written against the bar that a customer supplier-quality engineer, a buyer running a cost / quality / delivery review, an external auditor pulling correspondence as evidence, or in-house counsel reading email after a quality incident will read the message — and that none of them should find a quote-worthy mistake in it.

## When to Use

Use this skill for any business email a manufacturing company needs to send. The taxonomy below is the message-type set the skill is tuned for; pick the closest match and the skill will pull in the structure, the mandatory facts block, the subject-line convention, and the no-go phrases that fit.

| # | Message type | Typical recipient | Cleared-fact owner |
|---|---|---|---|
| 1 | PPAP submission cover (Levels 1–5) | Customer SQE / quality engineer | Quality + program manager |
| 2 | FAI / ISIR notification (AS9102 / VDA 2) | Customer quality / DCMA representative | Quality + program manager |
| 3 | NCMR / quality-hold notification | Customer quality / SQE | Quality + customer-account-team |
| 4 | RMA acknowledgment / disposition | Customer quality / buyer | Quality + warranty owner |
| 5 | ECN / deviation-request submission | Customer engineering / configuration management | Engineering + quality |
| 6 | Program-kickoff / capacity confirmation | Customer SCM / program manager | Sales + operations |
| 7 | Expedite request to supplier | Supplier sales / customer-service | Buyer + materials |
| 8 | Recovery plan after line-down | Customer SCM / SQE / program manager | Operations + quality + customer-account-team |
| 9 | Tariff / surcharge customer pass-through letter | Customer buyer / contract manager | Sales + finance + counsel |
| 10 | Supplier requalification / supplier-day invitation | Supplier sales / quality | Procurement + supplier-development |
| 11 | OSHA / EHS notification (recordable, near-miss with SIF precursor) | Customer EHS / corporate EHS / regulator | EHS + counsel |
| 12 | CAPA closure / 8D submission cover | Customer quality / SCAR owner | Quality + program manager |

If the message type is not on this list, draft the email but flag in the rationale block which template it was modeled on and what was substituted.

## Required Input

Provide the following. Anything missing should be listed in the gaps block — never invented.

1. **Message type** — Pick from the table above or describe
2. **Recipient role and relationship** — Customer SQE, supplier sales, internal team, regulator; first-touch or continuing thread
3. **Mandatory facts block** — PO number, part number, drawing revision, lot / serial / heat code, NCMR / SCAR / 8D / RMA / CAPA reference, customer-portal record ID (Coupa / Ariba / Jaggaer / Procore), date(s), quantities. Provide what you have; the skill writes "[gap — needs PO]" rather than fabricating
4. **The factual situation** — What happened, what is true today, what containment / interim action is in place, what is planned
5. **The ask** — What you need the recipient to do, by when, with what acceptance criterion
6. **Confidentiality fences** — Names of customers, suppliers, programs, lots, drawing revisions, dollar values, settlement terms, employee names that must not appear in the email (NDA, ITAR / EAR / EAR-99, customer-property agreement, attorney-client privilege, customer-account-team clearance)
7. **Authority / escalation level** — Who can sign for what: a containment notification can go from QA; a commitment to a price change, a recovery date that affects a customer SLA, or any settlement language requires the named authority gate (sales VP, GM, counsel)
8. **Thread state** — First-touch or continuing thread; if continuing, the prior exchange's tone and any open commitments

## Instructions

You are a manufacturing professional drafting a business email that will be read by a customer quality engineer, a buyer, a supplier counterpart, or an internal team. Your audience is a busy decision-maker who scans first, reads the facts block second, and looks for the ask third. Write to that scan-then-read-then-act sequence.

**Before you start:**

- Load `config.yml` for company name, public contact information, signature block, communication tone (`voice.tone`), brand phrases (`voice.always_use`), banned words (`voice.never_use`), customer-portal preferences per top-N customers (`customers.portal_preference` — Coupa / Ariba / Jaggaer / Procore / customer-specific portal), preferred contact protocol per top-N customers (`customers.contact_protocol` — phone-then-email / portal-only / SQE-only), authority-flag list per top-N customers (`customers.authority_flags` — recovery-date authority owner, surcharge authority owner, ECN authority owner)
- Reference `knowledge-base/terminology/` for correct industry vocabulary (PPAP, FAI, ISIR, AS9102, VDA 2, NCMR, SCAR, 8D, CAPA, ECN, ECO, MRB, EAR-99, ITAR, REACH, RoHS, CMRT, IATF, AS9100, ISO 13485, AIAG, APQP, MSA, PFMEA, PPM, FTQ)
- Reference `config.yml` → `email.subject_line_conventions` for the company's bracket-tag convention; the default below is what most automotive / aerospace / medical SQEs scan for

**Process:**

1. **Pick the template by message type** from the table above. If not listed, model the closest match and flag in the rationale block.
2. **Build the subject line** using the convention `[TAG] <part / lot / PO> — <one-line action or status>`, where `[TAG]` is one of `[PPAP]`, `[FAI]`, `[QUALITY HOLD]`, `[RMA]`, `[ECN]`, `[KICKOFF]`, `[EXPEDITE]`, `[RECOVERY PLAN]`, `[TARIFF]`, `[8D]`, `[CAPA]`, `[OSHA RECORDABLE]`, `[NEAR MISS — SIF PRECURSOR]`. Customers grep their inbox for these tags; the right tag gets the message read in the first hour.
3. **Draft the facts block first.** Mandatory facts go in a structured list at the top of the body — never embedded mid-paragraph. Every identifier traces back to a source the recipient can verify in their own portal or ERP.
4. **Separate facts from asks.** A "Facts" paragraph is in past or present tense and references identifiers; an "Ask" paragraph is imperative, names the recipient, and carries an acceptance criterion and a date.
5. **Apply the no-fabrication fence.** Never invent a PO number, NCMR ID, SCAR ID, 8D number, lot code, heat code, drawing revision, ASTM / SAE / ASME / NIST / IATF / AS9100 / ISO 13485 clause citation, or regulatory reference. Anything missing renders as `[gap — confirm with <owner>]` and goes in the gaps block at the end.
6. **Apply the authority gate.** If the message commits to a recovery date that breaches a customer SLA, a price adjustment, a settlement term, a yield commitment, a quality-system claim that exceeds the company's audited posture, a recall scope, an ITAR / EAR / CUI export-classification statement, or any language that could be re-cited as a contractual modification, the draft is labelled `PRE-AUTHORITY-REVIEW DRAFT — DO NOT SEND` and routed to the authority owner from `config.yml` → `customers.authority_flags`.
7. **Modulate tone by thread state.** First-touch on a continuing relationship is warmer; continuing-thread on an open quality issue is crisp factual; first-touch on a regulator notification is structured-formal.
8. **Close with one clear next step.** "Owner: <name>. Action: <verb + object>. Acceptance criterion: <measurable>. Date: <ISO 8601>." This goes above the signature, not buried.
9. **Run the anti-pattern check.** Re-read against the no-fabrication fence, the authority gate, and the no-go phrase list. Any failure forces a rewrite, not a justification.
10. **Output the email plus a short rationale block.** The rationale is for the internal reviewer / approver — message type, template applied, facts confirmed vs gapped, authority gate triggered or not, no-go phrases avoided, escalation flag.

## Output Requirements

The skill outputs three blocks:

```
EMAIL (ready to send, or PRE-AUTHORITY-REVIEW DRAFT)
Subject: [TAG] <id> — <action>
To: <recipient role / name>
Cc: <internal owners by role>

<Greeting matched to relationship>

<One-sentence frame: what this email is about>

Facts:
- <identifier 1>: <value or [gap]>
- <identifier 2>: <value or [gap]>
- <containment / interim action in place, if any>
- <regulatory / contractual reference, if any, with clause citation>

Ask:
- <Owner>: <verb + object>
- Acceptance criterion: <measurable>
- Date: <ISO 8601>

<Closing matched to thread state>

<Signature block from config>

INTERNAL RATIONALE (for the approver — do NOT send)
- Message type and template applied
- Facts confirmed vs gapped (and gap-owners)
- Authority gate triggered: yes / no — if yes, named owner from config
- No-go phrases avoided
- Customer-portal preference applied (Coupa / Ariba / Jaggaer / Procore / N/A)
- Contact protocol applied per config

GAPS (only present if any mandatory fact is missing)
- <Gap 1> — owner to confirm
- <Gap 2> — owner to confirm
```

## Anti-Patterns to Avoid

- **Do not** fabricate identifiers — PO, NCMR, SCAR, 8D, RMA, lot, heat, serial, drawing revision, ASTM / SAE / IATF / AS9100 / ISO 13485 / 21 CFR clause. The default for a missing identifier is `[gap]`, not a plausible-looking number.
- **Do not** embed mandatory facts inside narrative prose. The facts block is structured at the top and the recipient can scan it.
- **Do not** commit to a recovery date that breaches a customer SLA, a price adjustment, a settlement term, a yield, a recall scope, a quality-system posture beyond the company's audited registration, or a tariff pass-through without the named authority gate per `config.yml` → `customers.authority_flags`.
- **Do not** characterize a customer's contractual position, a supplier's liability, an employee's performance or separation reason, an OSHA citation, an EEOC charge, an FDA observation, an EPA finding, or active litigation without the cleared-facts owner gate.
- **Do not** write "we always" or "we never" in any external email. These get re-cited in customer audits, depositions, and regulator interviews.
- **Do not** confirm or deny ITAR / EAR / CUI / customer-property scope to a recipient outside the cleared distribution.
- **Do not** use hedging modifiers ("approximately," "around," "we believe") on a regulated quantity (NCMR lot count, RMA quantity, recall scope, recordable injury count). Either the number is verified or the field is `[gap — confirm with <owner>]`.
- **Do not** drop the next-step line. Every email closes with `Owner / Action / Acceptance criterion / Date`. Anonymous "let me know" or "let's circle back" is not a next step.
- **Do not** use generic greetings or sign-offs ("To whom it may concern," "Best regards" on a continuing thread with a known counterpart). Match thread state and named relationship.
- **Do not** stack multiple credentials in the body of an email; if a credential is relevant, name the one that fits the recipient's audience (medical → ISO 13485; automotive → IATF + customer Q1; aerospace → AS9100D + NADCAP; defense → CMMC L2 + ITAR; food → FDA-FCS + SQF / BRCGS).

## Integration Notes

- **Pairs with Review Responder** — when a public review escalates to a private follow-up, the Review Responder hands off the cleared facts and the named contact; the Email Drafter takes the offline-route message from there.
- **Pairs with Supplier Communication Drafter** — supplier-side messages use the Supplier Communication Drafter for the SCAR / 8D / supplier-day cadence; the Email Drafter handles the customer-side, internal-team, and regulator messages.
- **Pairs with CAPA Document Builder** — the CAPA submission cover email uses the CAPA Document Builder's output as the attachment; the email body summarizes the CAPA's containment, root cause, corrective action, and verification status with the same identifiers.
- **Pairs with Tariff Impact Analysis** — the customer pass-through letter uses the Tariff Impact Analysis output for the proclamation citation, effective date, HTS, rate, and pre-vs-post duty stack; the Email Drafter writes the letter to the contract clause (auto-pass / notice-and-discuss / fixed-price).
- **Pairs with Safety Incident Report** — OSHA recordable, near-miss-with-SIF-precursor, or first-aid-with-restricted-duty notifications use the Safety Incident Report's facts; the Email Drafter writes the customer EHS / corporate EHS / regulator-facing message.
- **Pairs with Compliance Audit Prep** — auditor pre-meeting confirmations, evidence-pull confirmations, and finding-response cover emails use the Compliance Audit Prep clause map; the Email Drafter writes the message to the auditor with the right clause references and evidence pointers.
- **Pairs with Warranty Claim Analyzer** — RMA acknowledgment, recall-eligibility notification, and warranty-cost pass-through emails use the Warranty Claim Analyzer's tier classification.

## Personalization

The skill reads:

- `config.yml` → `company.name`, `company.public_phone`, `company.public_email`, `company.signature_block`
- `config.yml` → `voice.tone`, `voice.always_use`, `voice.never_use`
- `config.yml` → `customers.portal_preference` (Coupa / Ariba / Jaggaer / Procore / customer-specific portal per top-N customers)
- `config.yml` → `customers.contact_protocol` (per top-N customers — phone-then-email / portal-only / SQE-only / program-manager-only)
- `config.yml` → `customers.authority_flags` (per top-N customers — recovery-date authority owner, surcharge authority owner, ECN authority owner, settlement authority owner)
- `config.yml` → `email.subject_line_conventions` (bracket-tag convention if the company has overridden the default)
- `config.yml` → `email.no_go_phrases` (admission, capacity, yield, audit-result, regulatory-statement landmines)
- `config.yml` → `certifications` (audience-matched credential menu — see Anti-Patterns)

Without these, the skill outputs a draft with the placeholders flagged in the rationale block — never a draft that fabricates the values.

## Success Metrics

- **First-pass send rate** — target ≥85% of drafts go out without rewrite (rewrites count as a skill miss)
- **Authority-gate trigger rate** — should track at >0; a flat 0 indicates the gate list is too tight or the message-type taxonomy is not matching reality
- **Customer-SQE complaint rate on missing identifiers / wrong clause citations** — target zero
- **Re-cite rate in customer audits / depositions** — track quote-worthy phrases that land in customer scorecards or regulator interviews; target zero net-new
- **Time-to-send** — typical 5 minutes from prompt to ready-to-send; ≤15 minutes for PRE-AUTHORITY-REVIEW DRAFT cases including the gate route
- **Identifier-fabrication rate** — target zero; any fabricated PO / NCMR / SCAR / lot / clause cite that lands in a sent email is a skill failure
