# Nga Phan: Delivery Coordinator, Role Definition and Onboarding

- **Title:** Delivery Coordinator (plain "Coordinator" internally; "Delivery Coordinator" on customer materials and email signatures)
- **Reports to:** Tu Thai, Solution Architect
- **Works with:** Duy Phan (Principal Firmware Engineer), Jun Sato (Business Strategist)
- **Employment:** Full-time, flexible remote working
- **Offices:** Tokyo (HQ) and Ho Chi Minh City (R&D and engineering hub)
- **Status:** Draft for discussion with Nga. Items marked *(to agree)* are proposals.

## 1. Purpose

Nga owns delivery at Gnomons. She turns agreed scope into plans, schedules, and clear status, keeps risks visible, makes sure work passes a quality gate before it reaches the client, and is the clients' day-to-day contact from kickoff through handover.

The role exists so that Tu, Duy, and Jun spend their time on architecture, firmware, and strategy instead of coordination. Nga does not make technical design decisions, and she is not support staff. She owns how the work is run.

## 2. Scope

**Owns**
- Project plans, milestones, and schedules for the projects assigned to her
- Risk and issue log, with escalation when a risk needs a decision
- Weekly written status for the team and, where relevant, for clients
- Client coordination day to day: questions, change requests, acceptance, handover
- Requirements and change tracking, so scope changes are recorded and agreed
- The delivery quality gate: definition of done, documentation, and test evidence complete before handover
- The team's lightweight process: backlog, status rhythm, risk log, meeting cadence

**Supports**
- Testing services: coordinating test plans, acceptance criteria, and test reports with the engineers who run them
- Jun's business side: turning agreed deals into scoped, staffed, scheduled projects
- Tu's scoping work: turning a client brief into milestones and deliverables

**Does not own**
- Architecture and design decisions (Tu)
- Firmware design, implementation, and technical estimates (Duy)
- Market strategy, pricing, and partnership decisions (Jun)

## 3. Decision rights

| Decision | Nga decides | Nga recommends | Others decide |
| :--- | :--- | :--- | :--- |
| Schedule and milestone dates on her projects | Yes, after estimates from Duy | | Tu if a date changes a client commitment |
| Status format, meeting cadence, tracker structure | Yes | | |
| Risk log entries and priority | Yes | | |
| Escalating a risk or blocker | Yes, to Tu | | |
| Client communication on her projects (routine) | Yes | | |
| Client communication on scope, price, or commitments | | Yes | Tu and Jun |
| Change request accepted or declined | | Yes | Tu (with Jun if commercial) |
| Passing the delivery quality gate | Yes | | Duy for technical sign-off on the evidence |
| Technical estimates and design choices | | | Duy and Tu |
| Order of work across projects when priorities conflict | | Yes | Tu |

Review these rights at day 90 and widen them as she builds a track record.

## 4. First projects

### Project 1: Cyber Resilience by Design roadmap (main assignment)

- **Why this one:** It already has a backlog (3 provisions built, 2 in progress, 11 planned across EN 303 645 §5 and §6) and outside anchors (Japan's JC-STAR labeling and the EU Cyber Resilience Act). It is standards-driven, so she can learn it from documents and traceability, not by reading code.
- **Nga owns:** the plan, sequencing, dependencies, risk log, status, and progress reporting.
- **Duy owns:** the technical work and estimates.
- **Dependencies to track:** secure software updates (rollback, key management, encryption in transit) and input validation for network-facing input.
- **Starts:** day 31. Days 1-30 are for reading and observing.

### Project 2: Conversational Hand Gestures retrospective (warm-up)

- Observe and document how this R&D project actually ran: timeline, effort, priorities, and lessons.
- Deliverable: a short retrospective (one to two pages) that becomes a planning template for future R&D projects.
- Timing: days 1-30.

### Project 3: RA8 reference design generator (scoping, later)

- Starts around month 3-4, once she is settled.
- Nga turns the vision (a manifest-driven generator producing reference designs for IoT, Edge AI, and cyber resilience, with Zephyr and native FreeRTOS backends) into a phased plan: the first two or three reference designs, milestones, and module priorities.
- Architecture questions stay with Tu and Duy. Nga's job is to get them decided in a sensible order, not to answer them.
- The cyber resilience roadmap is a natural first customer of the generator, so her plan for one informs the other.

### Project 4: Website content backlog (later, if bandwidth allows)

- Ongoing publishing cadence for new project pages and capability briefs.
- Positioning and site conventions stay with Tu.

## 5. Success measures *(to agree)*

Review at day 90 and month 6.

1. **On-time delivery:** milestones on the cyber resilience roadmap are met, or re-planned early with the reason recorded.
2. **Status rhythm:** a written weekly status goes out on schedule, with risks and decisions needed listed at the top.
3. **Less coordination load on Tu:** by month 3, Tu no longer runs planning, tracking, or routine client follow-up on Nga's projects.
4. **Protected engineering time:** Duy's interruptions fall, because priorities are clear and his questions are batched.
5. **Traceable scope:** every scope change on her projects is recorded, agreed, and visible.
6. **Quality gate in use:** the definition of done exists and is applied before each handover.

## 6. Working agreement

- **Core overlap hours:** 3-4 hours a day *(to agree)* when everyone is reachable. Tokyo and Ho Chi Minh City are two hours apart, so this is easy to set. Outside core hours she can flex freely.
- **Outcomes over hours:** she is judged on the success measures, not on when she is online.
- **One home for each thing:** one chat channel, one project tracker, and one shared document space. Decisions and status live there, not in private messages.
- **Meetings:** weekly 1:1 with Tu, a weekly team sync with cameras on, and a brief daily async check-in during weeks 1-2.
- **Kickoff:** an in-person kickoff week if she is near Tokyo or Ho Chi Minh City. Otherwise, video 1:1s with Tu, Duy, and Jun in week 1.

## 7. 90-day plan

### Days 1-30: learn the context
- Week 1: kickoff with Tu (vision, service lines, customers), then 1:1 introductions with Duy and Jun.
- Read the architecture and project documents, the cyber resilience brief, and an overview of Renesas-based development.
- Join customer and internal meetings as an observer, with a short debrief with Tu after each.
- Two or three 45-60 minute sessions with Duy on how firmware work is planned and estimated. The aim is technical literacy, not coding skill.
- Write the Conversational Hand Gestures retrospective.

### Days 31-60: own something small
- Take over planning and tracking for the cyber resilience roadmap.
- Set up the lightweight team process: backlog, weekly written status, risk log, definition of done.
- Run the first weekly status cycle and adjust the format with feedback from Tu and Duy.

### Days 61-90: own delivery end to end
- Run one roadmap milestone through delivery, including client communication where relevant.
- Hold the 90-day review against the success measures in section 5.
- Adjust her scope and decision rights based on the review.

### Months 4-6: choose the direction
- Start scoping the RA8 generator and take on the website backlog if bandwidth allows.
- Decide with Tu whether she grows toward program or product ownership, or adds quality and test coordination. Use the first projects as the evidence.
- Consider whether the title should move to Senior Coordinator or Program Coordinator once she is running several projects.

## 8. Risks to manage

- **Duy's bandwidth:** both main projects depend on his time. Nga makes his workload visible, sets a clear priority order, and batches questions.
- **Technical credibility:** she builds it by making Duy's work easier and by structured exposure to the firmware work, not by matching his depth.
- **Overlap with Tu:** be explicit about which decisions move to her and when. If not, Tu will keep making them by default.
- **Remote isolation:** keep 1:1s and the weekly team sync going, and ask directly at each 1:1 what is unclear and what she needs.
- **Ramp pace:** a gradual ramp with regular check-ins works better than a fast one. Ask early and often, so problems surface early.
- **Title read as junior:** the decision rights in section 3 carry the seniority. Introduce her to clients and to the team as the person who owns delivery.

## 9. Open items

- [ ] Confirm the success measures and their targets with Nga.
- [ ] Confirm core overlap hours and her primary office.
- [ ] Confirm the katakana reading of her name for the website (ンガ・ファン).
- [ ] Choose the tracker and shared document space before day 1.
