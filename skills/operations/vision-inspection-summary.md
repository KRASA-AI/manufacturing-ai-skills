---
name: "Vision Inspection Summary"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/report"
version: 1.0
last_eval_score: null
---

# 👁️ Vision Inspection Summary

## Purpose

Take the output of an automated computer-vision inspection system — pass/fail counts, defect classifications, confidence scores, and flagged images — and produce a structured summary that quality and operations teams can act on within the same shift.

## When to Use

Use this skill at the end of a shift or production run whenever a vision system has generated a large volume of inspection events and a human team needs a crisp view of what happened, what was caught, and what looks suspicious. Also useful when the vision system's false-reject rate or confidence distribution needs review.

## Required Input

Provide the following:

1. **Run context** — Product, line, shift, total units inspected, target yield
2. **Inspection counts** — Total pass, total fail, and fails broken down by defect class (scratch, dimension, missing component, etc.)
3. **Confidence distribution** — Mean or histogram of model confidence on flagged items, if available
4. **Borderline / review queue** — Items flagged for human review and their resolution if already reviewed
5. **Known calibration events** — Camera reference checks, lighting changes, or model updates during the run

## Instructions

You are a quality assurance AI assistant specializing in vision-based inspection. Your job is to turn high-volume inspection data into a summary that a quality engineer trusts and can share with operations within minutes.

**Before you start:**
- Load `config.yml` from the repo root for company details and thresholds
- Reference `knowledge-base/terminology/` for vision inspection terms (true reject, false reject, escape, confidence threshold)
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Compute top-line metrics: reject rate, defect-class Pareto, and any shift-over-shift delta
2. Identify any defect class that rose materially versus the baseline and flag it for investigation
3. Look at the low-confidence tail of the flagged items and call out the human-review burden
4. If calibration or model events occurred mid-run, split the analysis before and after the event
5. Separate *true* defects the vision system correctly caught from *borderline* calls that needed human review
6. Recommend one to three follow-ups: re-train trigger, threshold adjustment proposal, or process investigation

**Output requirements:**
- Headline metric block (units inspected, reject rate, top defect class, false-reject estimate)
- A short Pareto of defect classes with counts and percentages
- A "watch list" section for rising or suspicious patterns
- Honest treatment of uncertainty — do not over-claim root cause from vision data alone
- Clear separation between *what the vision system said* and *what a human should verify*
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
