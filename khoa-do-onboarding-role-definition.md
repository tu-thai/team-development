# Khoa Do: Junior Engineer (AI / Testing / Tooling), Role Definition and Onboarding

- **Title:** Engineer (junior level, ~1 year of experience)
- **Reports to:** Tu Thai, Solution Architect (career, process, and overall direction)
- **Technical mentor:** Duy Phan, Principal Firmware Engineer (firmware/technical mentorship, code review, technical sign-off on Tracks 2-3; reviews Track 1 output). Tu directs Track 1 (embedded AI model conversion/quantization) specifically.
- **Works with:** Jun Sato (Business Strategist), Nga Phan (Assurance Engineer — see Track 2 overlap below)
- **Employment:** Full-time *(to confirm)*
- **Offices:** Tokyo (HQ) and Ho Chi Minh City (R&D and engineering hub). Khoa is based in Ho Chi Minh City, alongside Duy and Tu.
- **Working pattern:** Hybrid — 2 days/week in the Ho Chi Minh City office, 3 days remote. Khoa's office days already align with Duy's.
- **Starts:** Week of 2026-09-28
- **Status:** Draft for discussion with Duy and Khoa. Items marked *(to agree)* are proposals.

## 1. Purpose

Khoa is Gnomons' first junior engineering hire. Rather than a generalist rotation to "find his fit," he's being deliberately groomed across three specific tracks the team needs: **AI engineering** (model evaluation, accuracy, and conversion for edge deployment), **firmware testing**, and **tool development**. Each track is anchored in a real, existing project area rather than exercises, so his output is real work from day one, reviewed by Duy.

This is a multi-quarter grooming arc, not something to compress into 90 days. Treat the first 90 days as foundational exposure to all three tracks — enough to see where he's strongest and where the team most needs him — with depth in each building over the following quarters.

## 2. Scope

**Owns**
- Implementation tasks assigned to him within a track, once scoped and reviewed by Duy
- His own task tracking, questions log, and status updates in his 1:1s
- Documentation of what he learns/builds, so it's reusable (test notes, setup guides, small how-tos)

**Supports, across the three tracks**
- **Track 1 — AI engineering:** model evaluation and accuracy benchmarking on the Conversational Hand Gestures demo (vision + language workloads on RA8P1/µT-Kernel), moving toward model conversion/quantization work as he builds foundation
- **Track 2 — Firmware testing:** test cases, validation, and quality-gate evidence, starting with the Cyber Resilience by Design (EN 303 645) roadmap but not limited to it — any firmware testing Duy assigns falls under this track
- **Track 3 — Tool development:** content TBD, picked by Duy/Tu closer to the time — could be the RA8 reference design generator, or PC-side tooling (internal scripts, dev-workflow tools, etc.)
- **Nga's assurance work (Track 2 overlap):** Nga (Assurance Engineer) writes the EN 303 645 test specifications and gap assessments for the cyber resilience roadmap; Khoa runs the firmware-facing parts of that testing under Duy's direction and review. Nga defines what evidence is needed, Duy still owns the technical execution and sign-off.

**Does not own**
- Architecture and design decisions (Tu)
- Firmware architecture, technical estimates, and code review sign-off (Duy)
- Client communication and commitments (Tu, Jun)
- Market strategy and partnerships (Jun)

## 3. Decision rights

| Decision | Khoa decides | Khoa recommends | Others decide |
| :--- | :--- | :--- | :--- |
| How he implements an assigned task (within the spec) | Yes | | Duy reviews/approves |
| Task scope, technical approach, design tradeoffs (Tracks 2-3) | | Yes, on small pieces | Duy |
| Task scope and technical direction, embedded model conversion/quantization (Track 1) | | Yes, on small pieces | Tu directs, Duy reviews |
| What he works on next / rotation timing | | Yes | Tu and Duy |
| Code merged into a project | | | Duy (review and sign-off) |
| Estimates on his own tasks | | Yes, with Duy checking | Duy |
| Client-facing commitments | | | Tu, Jun |

Review and widen these at day 90.

## 4. Development plan: three tracks (first 90 days)

Each track gets real, well-scoped tasks from day one — no shadowing. The order below sequences the tracks rather than running them in parallel, so Khoa can build one habit before adding the next. Sequence: **Track 1 (AI) first, then Track 2 (Testing), then Track 3 (Tooling).**

### Weeks 1-4: Track 1 — AI engineering foundations (Conversational Hand Gestures)
- Dev environment already set up from prior experience with the team — skip setup, just confirm access to current repos and tools.
- Read up on the demo's vision + language pipeline on RA8P1/µT-Kernel.
- Khoa already has some ML/Python background (evaluation scripts, notebooks), so this track can move faster than a cold start — start with **model evaluation**: running existing accuracy benchmarks and interpreting results on real data, then move toward conversion/quantization sooner than a full novice would.
- **Different mentorship pattern for this track:** the embedded-specific part (model conversion/quantization for RA8P1) is taught project-based rather than by generic code review — Tu directs (sets the task, scope, and technical direction) and Duy reviews the output. Tracks 2 and 3 stay with Duy directing and reviewing.
- A first small conversion/quantization task under this Tu-directs/Duy-reviews setup, once evaluation work is comfortable — likely reachable within this window given his background.

### Weeks 5-7: Track 2 — Firmware testing
- Read the architecture docs and the cyber resilience brief for context, since the EN 303 645 roadmap is the likely starting point.
- Small, bounded testing tasks picked by Duy — starting with the cyber resilience roadmap, where increasingly these will be the firmware-facing parts of test specifications that Nga (Assurance Engineer) writes, rather than tasks invented from scratch, since her Phase 1 work covers the same roadmap. Duy can also assign any other firmware testing task as it comes up; this track isn't limited to that one roadmap.
- Duy still directs and reviews Khoa's technical work here; Nga defines the evidence needed and consumes the result, but does not direct or review Khoa's work.
- Goal: build the review habit and testing discipline, now under Duy directing and reviewing directly (a shift from the Track 1 pattern above).

### Weeks 8-10: Track 3 — Tool development (content TBD)
- Content not yet decided — Duy and Tu pick the specific task closer to the time. Candidates include the RA8 reference design generator (manifest-driven platform) or PC-side tooling (internal scripts, dev-workflow tools, build/test automation), or something else entirely.
- Whatever is picked, keep it a small, bounded feature or task, reviewed by Duy.
- Goal: comfort with whichever tooling conventions and codebase Duy assigns, at a shallow level.

### Weeks 11-13 (days ~75-90): review across all three tracks
- No single "pick a direction" — the goal is breadth across AI, testing, and tooling by day 90, not narrowing to one.
- Hold the 90-day review (below), assess relative strength/interest across the three tracks, and use that to weight (not eliminate) tracks for the next quarter.
- AI engineering may actually be his strongest track early on given his existing ML/Python background — don't assume it's the weakest by default; let the review decide.

## 5. Success measures *(to agree)*

Review at day 90.

1. **Codebase and conventions fluency:** comfortable navigating the team's repos, git workflow, and review process independently.
2. **Track 1 — AI foundations:** understands the gesture demo's model evaluation pipeline and can run/interpret accuracy benchmarks independently; given his existing ML/Python background, aim for a first reviewed conversion/quantization task by day 90, not just exposure.
3. **Track 2 — Testing:** can write/run firmware test cases (starting with the cyber resilience provisions, but any Duy assigns) and produce quality-gate evidence with minimal hand-holding.
4. **Track 3 — Tooling:** has shipped at least one small feature or task on whatever tooling work Duy/Tu assign (scope TBD), reviewed by Duy.
5. **Review quality:** shrinking number of review round-trips with Duy across the period, not just task count.
6. **Low mentoring drag:** Duy's mentoring time is bounded and predictable (see Working agreement), not an open-ended interruption source.

## 6. Working agreement

- **Mentoring time:** Duy blocks dedicated time for code review and questions (*(to agree)* — e.g. a fixed daily/weekly slot) rather than ad hoc interruptions, to protect his own project time. No shadowing — Khoa works his own assigned tasks and brings questions/output to this slot. Their aligned office days are a natural anchor for this, especially in weeks 1-2, and use scheduled calls on remote days.
- **Track 1 specifically:** Tu blocks separate time to direct the embedded model conversion/quantization work (task, scope, technical direction); Duy still reviews the resulting output. Keep this distinct from the Track 2-3 mentoring slot so Khoa knows which person to bring what to.
- **1:1s:** weekly 1:1 with Tu (career, process, how it's going) and a separate weekly check-in with Duy (technical progress, blockers). Since Tu, Duy, and Khoa are all based in Ho Chi Minh City, these can be in-person on shared office days rather than remote by default.
- **Daily async check-in** during weeks 1-2 while he ramps up.
- **Questions:** batch non-urgent technical questions for the scheduled Duy slot; escalate blockers immediately.
- **One home for tasks:** tracker/shared doc space still to be decided — there's no delivery coordinator maintaining one, since that role was dropped when Nga moved to Assurance Engineer. Tu to decide where Khoa's tasks live (see open items).

## 7. Risks to manage

- **Duy's bandwidth:** mentoring a junior engineer competes directly with Duy's own delivery and architecture work. Bound the mentoring time explicitly (see Working agreement) instead of letting it become open-ended.
- **Three tracks is a lot for one junior engineer:** AI engineering, testing, and tooling are each substantial disciplines on their own. 90 days buys foundational exposure to all three, not mastery of any — set that expectation with Khoa explicitly so he isn't measuring himself against a senior bar on any single track.
- **AI track needs specialized teaching, not just code review:** model evaluation and conversion/quantization aren't things Khoa will pick up from generic review the way testing or tooling conventions might. Resolved by splitting the roles for Track 1 specifically: Tu directs (task, scope, technical direction) and Duy reviews the output — a different pattern than Tracks 2-3, where Duy does both. Worth watching that this split stays clear to Khoa so he knows who to bring what to.
- **Unscoped tasks:** junior + open-ended task is a bad combination. Duy or Tu scope each task down to something concretely reviewable before handing it over.
- **Hybrid pairing gaps:** office days are aligned across Tu, Duy, and Khoa now, but if that ever drifts, pairing falls back to remote/async only. Worth checking this stays true as schedules change.
- **No one owns coordinating Khoa's work across three tracks:** with the Coordinator role dropped, nobody is explicitly tracking/scheduling his ramp the way a delivery coordinator would have. Tu (career/process) and Duy (technical) split this informally today — fine for a self-managed team, but worth watching once Track 2 work with Nga adds a third input.

## 8. Open items

- [ ] Confirm employment type (full-time) and exact start date.
- [ ] Tu and Duy to pick the first concrete task for week 1 (Track 1: AI model evaluation).
- [ ] Agree Duy's dedicated mentoring time block.
- [ ] Decide tracker/shared doc space for his tasks — no longer Nga's setup, since coordination is out of her scope now. Tu to decide (self-managed team, per Nga's operating principle).
- [ ] Confirm katakana reading (proposed: クァ・ド) and whether/when to add him to the public website team page.
- [ ] Decide what Track 3 (tool development) actually is — RA8 generator, PC-side tooling, or something else — before weeks 8-10 arrive.
