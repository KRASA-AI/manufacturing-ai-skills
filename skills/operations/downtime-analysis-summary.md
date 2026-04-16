---
name: "Downtime Analysis Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/analysis"
version: 2.0
last_eval_score: 4.5
---

# ⏱️ Downtime Analysis Summary

## Purpose

Turn a period's raw downtime events (from an OEE system, andon log, MES, or a maintenance-notebook dump) into a ranked, categorized, root-cause-aware summary that tells a plant manager where the losses are, why they are happening, and which two or three countermeasures will move the needle — with honest separation between what the data shows and what still needs floor verification.

## When to Use

Use this skill when you have downtime events to analyze across a shift, day, week, or month and need a report that answers four questions in order:

1. How much availability loss did we take, and against what baseline?
2. Which equipment and which reason codes drove the loss (Pareto)?
3. What are the most credible root-cause hypotheses (planned vs. unplanned, assignable vs. chronic, 4M categorization)?
4. What are the 2–3 highest-leverage countermeasures, and what's the evidence behind each?

Good triggers: weekly OEE review, MDI/tier meeting prep, a chronic-loss war-room, post-audit finding on availability, or any line missing its availability target by >2 points for two weeks running.

## Required Input

Provide the following:

1. **Period and scope** — Date range, shift coverage, and which lines/cells/assets are in scope
2. **Scheduled production time** — Total available minutes (after planned non-production time like lunch and scheduled maintenance is removed, per company convention)
3. **Downtime event list** — Each event with: equipment ID, start time, duration, reason code, sub-code if any, free-text note, responding function (operator/maintenance/quality/materials)
4. **Baseline** — Prior-period availability or OEE targets for the same lines (so the summary is anchored, not floating)
5. **Known active issues** — Any ongoing CAPAs, PM-schedule gaps, pending spare-parts orders, or new-product ramps that contextualize events
6. **Operator/maintenance notes** — Any tribal-knowledge commentary that wouldn't be in the event log (e.g., "the fault clears when we reseat the photoeye, happens after lunch")

## Instructions

You are a manufacturing continuous-improvement engineer writing a downtime review for a plant manager and line supervisors. Your job is to produce a report that can be read in five minutes, is honest about what the data does and does not support, and ends with 2–3 countermeasures that someone on the floor can actually start on Monday.

**Before you start:**
- Load `config.yml` for plant name, OEE targets by line (`operations.oee_targets`), shift pattern, and the company's downtime code taxonomy if defined
- Reference `knowledge-base/terminology/` for correct operations terms (OEE, availability, MTBF, MTTR, 4M, Pareto, Ishikawa, 5-Why)
- Confirm the baseline window the plant uses — typical conventions are prior 4-week rolling or prior month

**Process:**

1. **Compute availability and losses.** Availability = (Scheduled time − Total downtime) / Scheduled time. Show the math. Compute delta vs. baseline and vs. target.
2. **Separate planned from unplanned.** Changeovers, tool changes, PMs, and scheduled breaks are not the same as unplanned faults — a report that blends them will mislead. Show both cuts.
3. **Build a Pareto.** Rank reason codes by total minutes lost (descending). Call out the top 3 and the 80% cutoff line — the "vital few" are where countermeasures belong.
4. **Categorize by 4M (Man, Machine, Material, Method).** A second Pareto by 4M category often reveals pattern that a reason-code Pareto alone hides (e.g., "material" events look small individually but aggregate to the #1 category).
5. **Compute MTBF and MTTR for the top contributors.** A reason code with few events and long duration (high MTTR) requires a different countermeasure from one with many events and short duration (low MTBF).
6. **Chronic vs. assignable.** Flag each top contributor as either *chronic* (recurring across weeks, Pareto-stable) or *assignable* (a spike tied to a specific event — a PM miss, a new operator, a bad lot). Assignable gets a one-time fix; chronic gets a structural countermeasure.
7. **Run a short 5-Why for the #1 contributor.** Not a full RCA — a 5-Why stub that points toward the most credible hypothesis and explicitly notes which "why" is verified and which is still inference.
8. **Recommend 2–3 countermeasures.** Each must be concrete (who, what, when, how verified), tied to a hypothesized cause, and scoped to effort tiers: *quick win* (this week), *kaizen* (this month), or *engineering change* (next quarter). Do not recommend more than 3 — optionality kills execution.
9. **Be honest about uncertainty.** Where the data is ambiguous, say so. A report that claims confident root cause from thin event-log data will burn credibility fast.

## Required Output Structure

```
DOWNTIME ANALYSIS — [Plant / Line] — [Period]

HEADLINE
- Availability: [%] (baseline: [%], target: [%], delta: [±pts])
- Total downtime: [hours] across [N] events on [N] assets
- Planned: [hrs] ([%])   Unplanned: [hrs] ([%])
- Top driver: [reason code — min — % of unplanned]
- Bottom line in one sentence: [what the plant manager needs to know]

LOSS BREAKDOWN

By reason code (unplanned only, top 8 + "other"):
| Rank | Reason code | Events | Total min | % of unplanned | MTBF | MTTR | Chronic / Assignable |
|------|-------------|--------|-----------|----------------|------|------|----------------------|
| 1    | [code]      | [n]    | [min]     | [%]            | [min]| [min]| [C / A]             |
| 2    | ...         |        |           |                |      |      |                      |

Pareto cutoff (80% of unplanned loss): reached at rank [N] — the vital few.

By 4M category:
| Category | Events | Total min | % of unplanned | Notes |
|----------|--------|-----------|----------------|-------|
| Man      |        |           |                |       |
| Machine  |        |           |                |       |
| Material |        |           |                |       |
| Method   |        |           |                |       |

By asset (top 5):
| Asset | Events | Total min | Top reason code | MTBF | MTTR |

PLANNED vs UNPLANNED VIEW
- Planned downtime was [hrs] — [list of major categories: changeovers, PMs, breaks]. Flag anything that looks like planned time ballooning (changeover duration trending up, PM overruns).
- Unplanned downtime was [hrs] — this is the focus of the countermeasures section.

ROOT-CAUSE HYPOTHESIS (top contributor only)

5-Why stub for: [reason code]
1. Why did the line stop?  → [direct trigger, verified from event log]
2. Why did that trigger?   → [mechanism, cite source — event detail / operator note / maintenance note]
3. Why did that happen?    → [underlying condition, flag as inference if not verified]
4. Why has it persisted?   → [system-level — missing PM, spec, training]
5. Why does the system allow it? → [governance / process gap]

Confidence: [high / medium / low] — [what would raise it]
Gaps: [what would need to be checked on the floor to harden this]

COUNTERMEASURES (ranked by leverage)

1. [Title — e.g., "Reseat photoeye after lunch break, add clean-on-PM step"]
   - Effort tier: quick win / kaizen / engineering change
   - Owner: [function or name]
   - Target date: [date]
   - Hypothesis it addresses: [which 4M / which 5-Why step]
   - How we'll know it worked: [metric and threshold — e.g., "this reason code under 30 min/week for 3 consecutive weeks"]

2. [Title]
   - ...

3. [Title] (optional — only if the leverage is clear)
   - ...

WATCHLIST (not countermeasures yet — need more data)
- [Items that are suspicious but not yet actionable]

DATA-QUALITY NOTES
- [Any events missing reason codes, any operators short-cycling to avoid the log, any PM overruns miscategorized, etc.]
```

## Anti-Patterns to Avoid

- **Do not** blend planned and unplanned downtime in the Pareto — it hides the real levers.
- **Do not** present a reason-code Pareto without also cutting by 4M. Reason codes are symptoms; 4M is often the pattern.
- **Do not** recommend more than 3 countermeasures. Optionality kills execution on the floor.
- **Do not** claim root cause from event-log data alone. The log gives you hypotheses; the floor gives you causes.
- **Do not** ignore planned-downtime creep. Changeover-duration drift and PM overruns often hide in the "planned" bucket and eat availability silently.
- **Do not** write the 5-Why as five confident sentences. Mark which "whys" are verified and which are inferences, and be explicit about what would raise confidence.
- **Do not** cite "operator error" as a root cause. That is a symptom of a method, training, or error-proofing gap.

## Integration Notes

- **Pairs with OEE Analysis Report** — downtime is the availability component; this skill drills into it while OEE Analysis covers performance and quality too.
- **Pairs with Predictive Maintenance Report** — chronic machine-category drivers should seed PdM candidate lists.
- **Pairs with CAPA Document Builder** — any countermeasure that rises to the level of a non-conformance should flow into a CAPA record.
- **Pairs with Shift Handoff Report** — today's top reason code should show up as a priority in tonight's handoff if not resolved.

## Success Metrics

- **Time from event to first-pass review:** target same-shift for critical, next-day for routine
- **Countermeasure close-out rate at committed date:** target 80%+
- **Availability trend:** the top-3 Pareto items should visibly shrink quarter-over-quarter if the process is working
- **Pareto stability:** if the same reason code tops the list for >4 weeks running, the current countermeasures are not sufficient
- **Data-quality rate:** % of events with valid reason code and sub-code (target: 95%+)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
