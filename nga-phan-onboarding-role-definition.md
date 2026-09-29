# Nga Phan: Assurance Engineer, Role Definition and Onboarding

- **Title:** Assurance Engineer (website label: Assurance)
- **Reports to:** Tu Thai, Solution Architect
- **Works with:** Duy Phan (Principal Firmware Engineer), Jun Sato (Business Strategist), Khoa Do (Junior Engineer)
- **Employment:** Full-time, flexible remote working
- **Offices:** Tokyo (HQ) and Ho Chi Minh City (R&D and engineering hub)
- **Status:** Draft for discussion with Nga. Items marked *(to agree)* are proposals.

## 1. Operating principle

Gnomons stays light and self-managed. Every team member owns their own work, and important decisions stay with Tu. Nga is an individual contributor: she produces assurance deliverables herself. She does not coordinate, manage, or gate anyone else's work.

## 2. Purpose

Nga leads compliance and assurance at Gnomons. She turns regulations and standards into concrete work products (gap assessments, test specifications, conformance evidence, and technical documentation) for Gnomons' own reference designs and as a service for clients.

The gap she fills: Duy and Khoa build the protections and features, and Jun sells them. Nobody currently owns proving that a product meets the rules, which is what clients need to reach JC-STAR, CRA, and AI Act conformity.

## 3. Scope by phase

### Phase 1 (months 0-6): Cybersecurity compliance

- **EN 303 645:** gap assessments and a test specification for each provision, mapped to the cyber resilience roadmap (3 built, 2 in progress, 11 planned across §5 and §6)
- **JC-STAR:** Japan's IoT labeling scheme, which builds on EN 303 645. She maps its requirements and prepares the self-assessment evidence.
- **EU Cyber Resilience Act:** technical documentation, SBOM (software bill of materials) upkeep, and vulnerability handling and disclosure procedures. The CRA's vulnerability reporting obligations have applied since 11 September 2026. Full application follows on 11 December 2027.
- **Light QA:** test specs, traceability matrices, verification report templates, and checklists for her own deliverables. Others can reuse them if they choose.

### Phase 2 (months 6-12): AI compliance and ethics

- **EU AI Act:** classify client use cases by risk level and prepare model documentation and data-provenance records. After the AI Omnibus (in force 27 July 2026), stand-alone high-risk obligations apply from 2 December 2027. High-risk AI embedded as a safety component in products applies from 2 August 2028. SME relief measures may apply to Gnomons and should be checked.
- **Japan:** the AI Promotion Act (innovation-first, no monetary penalties) and the AI Guidelines for Business, which clients are most likely to ask about.
- **ISO/IEC 42001:** track the AI management system standard and adopt it only if clients start asking for it.
- **Responsible AI:** privacy by design, transparency, and care with vulnerable users, built into the AI work rather than run as a separate track.

### Year 2: Package the service

- Decide with Tu and Jun whether cyber and AI assurance become one client-facing service.

### Out of scope

- **Functional safety (IEC 61508, ISO 26262):** a specialist field with its own certification path. Take it on only if a client needs it, and then probably with an external partner.
- **Coordination:** she does not manage schedules, estimates, or delivery for others.
- **Gatekeeping:** QA means supplying evidence and templates, not approving other people's work before it can ship.

## 4. Decision rights

| Decision | Nga decides | Nga recommends | Others decide |
| :--- | :--- | :--- | :--- |
| How she plans and runs her own assurance work | Yes | | |
| Structure of assessments, test specs, and evidence files | Yes | | |
| Interpretation of a standard for an internal deliverable | Yes | | Tu if it changes a design |
| Whether a provision is met, based on the evidence | Yes | | Duy confirms the technical facts |
| Conformance claims made to clients or on the website | | Yes | Tu |
| Adding a new regulation or standard to the scope | | Yes | Tu |
| Pricing and packaging of assurance services | | Yes | Tu and Jun |
| Design changes needed to close a gap | | Yes | Tu and Duy |
| How Duy, Khoa, or Jun run their own work | | | Themselves |

## 5. First deliverables

1. **Obligations map (days 1-30):** what the CRA, JC-STAR, and (later) the AI Act require of Gnomons as an engineering partner, compared with what they require of our clients as manufacturers. This anchors everything else.
2. **EN 303 645 gap assessment of the RA8P1 cyber resilience reference (days 31-60):** status of each provision, the evidence available, and gaps, linked to the roadmap.
3. **Test specifications for the 3 built provisions (days 31-90):** secure storage, minimized attack surface, and software integrity. Duy or Khoa run the parts that need firmware work.
4. **CRA documentation starter kit (days 61-90):** a technical file template, an SBOM process, and a vulnerability handling and disclosure procedure, usable for Gnomons and for clients.
5. **Responsible-AI case study (Phase 2):** the Conversational Hand Gestures device for children, which keeps all camera data on-device. It covers privacy by design and care with young users.

## 6. Success measures *(to agree)*

Review at day 90 and month 6.

1. **Evidence exists:** each built EN 303 645 provision has a test specification and a verification record.
2. **Claims are backed:** every conformance statement in the cyber resilience brief and on the website traces to evidence.
3. **Client-ready kit:** the CRA documentation starter kit can be used on a client engagement without rework.
4. **Low load on engineers:** Duy's and Khoa's time goes only to test runs that need firmware work, with requests batched.
5. **Regulatory currency:** the obligations map is updated whenever a relevant rule or deadline changes.

## 7. Working agreement

- **Core overlap hours:** 3-4 hours a day *(to agree)*. Tokyo and Ho Chi Minh City are two hours apart, so this is easy to set. Outside core hours she can flex freely.
- **Outcomes over hours:** she is judged on the success measures, not on when she is online.
- **One home for each thing:** one shared document space for assurance work, with evidence files versioned alongside the code where possible.
- **Meetings:** weekly 1:1 with Tu, the weekly team sync with cameras on, and a brief daily async check-in during weeks 1-2.
- **Kickoff:** an in-person kickoff week if she is near Tokyo or Ho Chi Minh City. Otherwise, video 1:1s with Tu, Duy, Jun, and Khoa in week 1.

## 8. 90-day plan

### Days 1-30: learn the rules and the products

- Week 1: kickoff with Tu (vision, service lines, customers), then 1:1 introductions with Duy, Jun, and Khoa.
- Read EN 303 645, the JC-STAR requirements, and the CRA's essential requirements.
- Read the cyber resilience brief and roadmap, and get an overview of Renesas RA8P1 and TrustZone.
- Two or three 45-60 minute sessions with Duy on how the built protections work. The aim is enough technical literacy to judge evidence, not coding.
- Deliver the obligations map.

### Days 31-60: first assessment

- Deliver the EN 303 645 gap assessment of the RA8P1 reference.
- Start test specifications for the 3 built provisions.

### Days 61-90: evidence and kit

- Complete the test specifications and the first verification records with Duy or Khoa.
- Deliver the CRA documentation starter kit.
- Hold the 90-day review against the success measures and confirm the Phase 2 start date.

### Months 4-12

- Extend evidence to the 2 in-progress provisions as they are built.
- Start Phase 2 (AI compliance and ethics) around month 6, beginning with the responsible-AI case study.

## 9. Risks to manage

- **Spreading too thin:** four regimes at once would stall her. Hold to the phases.
- **Drift into gatekeeping or coordination:** if assurance starts blocking or scheduling others' work, it has become the role you decided against. Pull it back early.
- **Regulatory churn:** deadlines have already moved (the AI Omnibus delay is an example). Date-stamp the obligations map and review it quarterly.
- **Overclaiming:** Gnomons can provide evidence and documentation, but conformity is the manufacturer's responsibility, and some cases need a notified body. Wording to clients must reflect that.
- **Technical credibility:** she builds it by producing evidence engineers find useful, and through structured sessions with Duy.
- **Ramp pace:** after a long break, a gradual ramp with regular check-ins works better than a fast one.

## 10. Open items

- [ ] Confirm the success measures and phase timing with Nga.
- [ ] Confirm core overlap hours and her primary office.
- [ ] Confirm the katakana reading of her name for the website (ンガ・ファン).
- [ ] Choose the document space and evidence file structure.
- [ ] Decide whether to budget for standards purchases or training (for example EN 303 645 or ISO/IEC 42001 courses).
- [ ] Check whether EU AI Act SME relief measures apply to Gnomons.
