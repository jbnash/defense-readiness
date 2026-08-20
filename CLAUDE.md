# Defense Readiness Check — build context

Read this before changing anything. It records decisions that were argued through and settled, so they don't get re-litigated or silently reversed.

**Owner:** John B. Nash, DGS, Educational Leadership Studies, University of Kentucky.
**Live:** https://jbnash.github.io/defense-readiness/
**Current version:** v14 (footer marker is the source of truth).

---

## What this is, and what it is deliberately not

A planning aid for UK doctoral students working out whether a target defense term is realistic. It answers **procedural** questions: NOTIF timing, 767 enrollment, committee distribution, the Request for Final Examination, off-contract conflicts, and the cost of defending at the term-end deadline.

It is **not** a substitute for the chair, committee, or DGS on substantive work, and it must never present itself as one. It is also not an official Graduate School product.

### The problem it exists to solve

A student was consuming enormous advisor time on procedural questions — deadlines, exceptions, whether he could defend — while the actual bottleneck was that his dissertation was still in the reconnaissance phase. The advisor was spending her energy on calendar mechanics instead of his research. This tool exists to absorb the procedural load so faculty time goes to the work.

That origin drives a design principle worth preserving: **the tool should not look too polished or automated.** A slick procedural answer can be mistaken for progress. The verdict, checklist, and next-actions are meant to land as *"the calendar isn't your bottleneck — the writing is."* Do not round that edge off.

---

## Rules encoded, and where they come from

Sources, in order of authority: the UK Graduate School *Policies & Procedures for Directors of Graduate Studies* (2024–25, rev. 10/17/24), the Registrar's academic calendars, and the EDL Doctoral Students SharePoint.

| Rule | Source | Notes |
|---|---|---|
| NOTIF at least 8 weeks before the exam | DGS manual | GS needs the time to audit the record and appoint an outside examiner (4–5 weeks of it) |
| Effective NOTIF deadline = **earlier of** (8 weeks before defense) or (published calendar deadline) | derived | This is the single most useful thing the tool does. In summer the calendar date is later than the 8-week date, so students who plan against the published number miss |
| Request for Final Examination, ≥2 weeks before the exam, after the outside examiner is appointed | DGS manual | A **second** form. Students routinely don't know it exists |
| Approved dissertation to the Outside Examiner ≥2 weeks before | DGS manual | Student's responsibility, not the chair's |
| Two semesters of 767 required before graduating | DGS manual | "Before they can graduate," not before the exam — see open questions |
| Continuous 767 every **fall and spring** until defense | DGS manual | Summer is not required. This is the summer-enrollment exemption |
| Academic-year defenses require 767 enrollment that term | DGS manual / GS Director | Hard bar |
| Final exams only while **classes are in session** | DGS manual | Kills the inter-term-gap workaround entirely. See below |
| Outstanding "I" or "S" grades in letter-graded courses block the exam | DGS manual | GS audits at NOTIF |
| Academic probation blocks the exam and the degree | DGS manual | Hard bar |
| Dissertation to GS within 60 days of the exam | DGS manual | Basis of the recommended defense date |
| Final ETD deadline assumes you made format review | Registrar PDF | The published wording is "for those students who first submitted [format review date]." Format review is the real deadline |
| Faculty off contract May 15 – Aug 15 | departmental practice | Not a GS rule; a scheduling reality |

### The classes-in-session finding

The manual permits final examinations only while classes are in session. Combined with the calendar, this means **there is no window that is both in session and before faculty go off contract** — summer session begins after May 15. A summer defense therefore requires either genuine off-contract faculty agreement or a DGS petition to the Graduate School Dean. There is no third option.

An earlier version of the summer caveat claimed students sometimes use the late-April/early-May inter-term gap for an August degree. **That was wrong and has been corrected.** Don't reintroduce it.

---

## Design decisions — settled, do not reverse without asking

**Recommended vs. latest defense date.** The tool plans toward a *recommended* date 60 days before the final ETD, so the full GS-permitted revision window is usable. The term's exam-sit deadline is shown as a badged backstop and explicitly labeled not advised — it leaves roughly 15 days for revisions. Rationale: deadline-driven planning produces rushed dissertations, unread committee members, and failed defenses.

**Lead-time model.** Milestones are *not* simply computed backward from the recommended date. `earliestFeasibleDefense = today + longest outstanding lead time` (chair approval 70d if not approved, distribution 56d if committee hasn't seen it, NOTIF 56d always). Planning slides to whichever is later, capped at the term's last exam date; overshoot means the term is unreachable. This exists because the naive version emitted milestone dates in the past.

**Verdict labels are developmental.** `ON TRACK` / `TIGHT` / `NOT YET` / `NOT THIS TERM`. There is no "NO" and no alarm red — the hardest verdict uses calm slate, because a term not working out is a scheduling fact, not a judgment on the student. Every hard-stop reason names the constraint and then opens a door.

**This paragraph is kept verbatim. Do not edit it:**

> The distance between here and a defense is writing, not paperwork. That's worth knowing plainly: no form or deadline will move faster than the manuscript does. ${term.label} isn't realistic, but the next term is a real target if the writing moves.

**Progress question is program-specific and concrete.** PhD asks about data collected and Ch. 4–5 written; EdD asks about evaluation data analyzed and results/monitoring/conclusions written. It replaced both an abstract "prerequisites" question and a generic draft-maturity scale. It lives under *Defense readiness*, not *Eligibility* — calling these "prerequisites" wrongly implied they were the only bar.

**Footer points to authoritative sources, not to itself.** DGS office phone first, then the DGS Manual, SharePoint, and Registrar's calendar. An earlier version said "use this tool first — before asking your chair or DGS." That was too presumptuous and was cut.

**Outstanding steps are scored against their own slack, not treated as blockers.** `leadStatus(slackDays)` tiers any not-yet-done step: ≥60 days of room → `todo`, ≥14 → `warn`, otherwise `miss`. It backs the chair-approval, committee-distribution, and outstanding-I/S-grades gates, each measured against the date that step is actually due (`chairApprovalBy`, `committeeDistBy`, `effectiveNotifDeadline`). Rationale: the lead-time model has *already* absorbed each step's duration, so a step with 494 days of runway is a scheduled action, not a failure. Before this, a defense-ready student who simply hadn't distributed yet got `NOT YET` for every term out to Summer 2028.

**`todo` is a fourth checklist status, rendered as a blue ○ (`[ ]` in plain text).** It is deliberately not the green ✓ — an outstanding item must not look complete — and deliberately not a warning. It counts toward neither `misses` nor `warns`, so it cannot move the verdict.

**Every unreachable verdict names the earliest term that does work.** `evaluate(input, probe)` takes a second argument; when falsy, the function walks `TERM_ORDER` re-entering itself with `probe = true` and returns the first term scoring `ON TRACK` or `TIGHT` as `earliestWorkingTerm`. The `probe` flag is the recursion guard — do not remove it. The line is rendered separately in `renderVerdict` (HTML) and `buildTextReport` (plain text); keep `result.reason` free of markup so the clipboard report stays clean.

**Enroll-in-767-and-drop-it is not a sanctioned path** and must never appear as one. It circulates as student folklore; the Graduate School does not endorse it.

---

## Architecture

Single self-contained HTML file, no build step, no dependencies. Open `index.html` in a browser.

- `TERMS` object near the top of the inline `<script>` holds all calendar data. Add a term there and add its key to `TERM_ORDER`.
- Planning constants sit just below: `REVISION_BUFFER_DAYS` 60, `NOTIF_LEAD_DAYS` 56, `CHAIR_LEAD_DAYS` 70, `DIST_LEAD_DAYS` 56, `REQUEST_EXAM_DAYS` 14, `OUTSIDE_COPY_DAYS` 14.
- `evaluate(input)` is the kernel. Gates are grouped into four categories: Eligibility, NOTIF readiness, NOTIF timing, Defense readiness. Verdict is computed from miss/warn counts plus hard blockers.
- Shareable URL state encodes form answers in the location hash; opening such a URL restores and re-runs automatically.
- Styling follows UK brand: UK Blue `#0033A0` dominant, UK Gold `#C1A875` accent, Georgia display, system sans body.

### Terms

Fall 2026 – Summer 2027 use the Registrar's published dates and are verified against the official AY 26–27 PDF. Fall 2027 – Summer 2028 are marked `provisional: true`: term anchors come from the official Five-Year Calendar, and the GS deadlines are projected from offsets that are identical across every published year (last class → exam sit: 6 days fall/spring, 14 summer; exam sit → schedule deadline: 14; exam sit → NOTIF: 76 fall / 63 spring / 39 summer; final ETD → format review: 7). Selecting a provisional term fires a banner. When the real AY 27–28 calendar publishes, replace the dates and remove the flag.

---

## Open questions and pending items

- **Term-independent `NOT YET` for early-stage progress.** `draftRank <= 1` (still collecting data, or earlier) returns `NOT YET` for *every* term in the window, including ones two years out, and `earliestWorkingTerm` therefore comes back null. This is partly deliberate — it is the origin-story principle that the writing, not the calendar, is the bottleneck — but the verbatim paragraph it prints says "the next term is a real target," which is incoherent when every term says it and wrong for the last term in the list. Needs a decision: keep as-is, or let the gate open once a term is far enough out. Not changed unilaterally because the paragraph is protected.
- **Can a student defend in the same term their second 767 completes?** The manual says two semesters are required "before they can graduate," not before the exam — which reads like yes. Currently coded as a warn with "confirm with your DGS." Resolve when known.
- **767 S/U grading:** whether the S grades must be consecutive, and what consequences follow from U grades. Christine at the Graduate School is looking into it; expected to land with the new ARs. Not encoded — don't guess.
- **International students** are not handled at all. The manual has a substantial section (F-1 students defending in spring aren't required to register for summer; those defending and submitting in summer should register for 767 or 748; those who defend in summer but haven't submitted by the fall SEVIS deadline must enroll full-time). Real trap for a population that can lose status. Would need a form question and a conditional results block.
- **Associate dean material** was referenced but never successfully attached. May contain further corrections.
- **Fall Break moves** starting AY 2027–28 — from late October to the Monday–Tuesday before Thanksgiving. Doesn't affect defense deadlines but changes November committee availability.
- Two published errors in the Registrar's Five-Year Calendar, worth reporting: the Fall Break row shows 2027 dates in the Fall 2028 and Fall 2029 columns, and Summer 2027's Independence Day is listed as 7/5/2026.

---

## Working style

Prefer verification over assertion — the date logic has been wrong twice in ways that only surfaced by running scenarios against a fixed "today." When changing the planning model, test with `today` set inside the term (e.g. 2026-08-18 for Fall 2026) and confirm no milestone lands in the past.
