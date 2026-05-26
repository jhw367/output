# Truviq health-market meeting readout — website, scoring, CRM automation

Updated: 2026-05-26 18:35 UTC

## Executive conclusion

The health-market focus should be **meeting setup and qualification**, not pitching “process orchestration” as an abstract technology. Truviq’s website already has credible building blocks — AI-driven process automation, Business Process Management, low-code, Pega, ServiceNow, Camunda, healthcare, government, Amsterdam location, FHIR/SMART-on-FHIR claims on the Camunda page — but the health buyer does not yet get a fast confirmation that Truviq understands Dutch/EU healthcare transformation, compliance, event-based meeting context, and remote delivery limits.

Recommended website message: **“We help healthcare teams turn transformation programmes into governed workflows across existing systems — handoffs, approvals, exceptions, monitoring and audit trails — starting with a 20-minute workflow-fit meeting.”**

## 1. Truviq website evidence checked

Sources checked:
- https://truviq.com/ — homepage.
- https://truviq.com/Services/digital-process-automation/ — digital process transformation.
- https://truviq.com/Services/intelligent-automation/ — intelligent automation.
- https://truviq.com/Services/legacy-modernization/ — legacy modernization.
- https://truviq.com/camunda/ — Camunda practice.
- https://truviq.com/resources/ — blogs, news and case studies.
- https://truviq.com/contact-us/ — locations, including Amsterdam.

Confirmed experience/positioning signals:
- Truviq says it has **10+ years of experience** in digital process transformation.
- It claims experience across **banking, financial services, insurance, healthcare and government sectors**.
- It positions around **AI-driven process automation**, **Business Process Management**, **low-code**, **Pega**, **ServiceNow** and **Camunda**.
- The Camunda page claims healthcare-relevant accelerators: **FHIR R4 connectors**, **SMART-on-FHIR OAuth2 security**, healthcare-grade workflows, BPMN linting and AI governance guardrails.
- Contact page lists **Amsterdam, Netherlands: Radarweg 29, 1043 NX, Amsterdam**.
- Resources include case studies in insurance, trust/fund management and banking, but no easily visible healthcare case study.

Health-market credibility gap:
- The site says “healthcare”, but it does not yet translate that into a Dutch/EU health buyer’s language: transformation funding, consent, identity, EHDS, NEN 7510, EHR/ECD integration, care-continuity, pathway onboarding, and accountable reporting.
- The main call to action is broad: “Start Your Automation Journey” / “Let’s Innovate Together”. For HLTH and Sales-People, the better call to action is a **low-friction meeting request**, not a solution pitch.

## 2. Website changes to confirm health-market match

### Must-have changes before health outreach

1. **Add a dedicated healthcare landing page**
   - Suggested title: `Healthcare transformation workflows — NL/EU`.
   - Plain-language subtitle: `Make handoffs, approvals, exceptions and audit trails work across existing healthcare systems.`
   - Avoid leading with “orchestration”; explain the operational problem first.

2. **Change the health/event call to action**
   - Primary button: `Book a 20-minute workflow-fit meeting around HLTH Europe Amsterdam`.
   - Secondary button: `Send one workflow to assess`.
   - Meeting framing: “We will identify the programme owner, system landscape, evidence gap and next step — no technical pitch unless there is fit.”

3. **Add six health use-case cards**
   - Hybrid-care pathway onboarding.
   - Digital identity, consent and access exceptions.
   - AI governance and shadow-AI control.
   - Acute-care handoffs: ambulance, emergency department, GP out-of-hours post.
   - Transformation-funding intake, monitoring and reporting.
   - Care-continuity and incident/fallback workflows around critical systems.

4. **Add remote-delivery proof**
   - Amsterdam/NL presence and EU time-zone coverage.
   - Remote discovery workshops.
   - No patient data required for first assessment.
   - Security posture: GDPR/AVG, NEN 7510-aligned language, data-minimisation, auditability.
   - 4–6 week proof-of-value format.

5. **Add health proof assets**
   - One anonymised healthcare case study or “healthcare workflow patterns” page if direct client names cannot be used.
   - A one-page “HLTH Europe workflow-fit meeting sheet”.
   - A short downloadable checklist: `Is this healthcare workflow ready for automation?`.

6. **Add a “not a fit” section**
   - Not for replacing EHR/ECD systems.
   - Not for generic innovation scouting.
   - Not for patient-facing clinical advice.
   - Good fit only when there are cross-system handoffs, exceptions, approvals, compliance evidence or operational monitoring gaps.

## 3. Elaborate lead scoring mechanism

Each account receives a stable unique ID and a 0–100 score. Current scoring file:

- Local file: `/home/hermes/work/truviq/markt-health/data/lead_scoring.csv`

Score dimensions:
- **Strategic fit — 18 points:** evidence of handoffs, workflows, governance, monitoring, reporting, auditability or cross-system process pain.
- **Account-type route — 15 points:** direct buyer highest, then implementation partner, ecosystem influencer, regulatory/tender monitor.
- **Trigger urgency — 15 points:** high/medium/low priority based on event timing, regulation, funding, incident pressure or operational pain.
- **Funding/procurement evidence — 15 points:** named programme, funding route, tender/procurement signal, model plan or budget clue.
- **Technology-stack evidence — 10 points:** named systems, standards or platforms such as ChipSoft HiX, FHIR, SMART-on-FHIR, LSP, Nuts, Mitz, ZORG-ID, UZI, EHR/EPD/ECD, portals, NEN 7510.
- **Buyer-persona clarity — 10 points:** likely programme owner, budget owner, implementation owner or procurement route.
- **Event-meeting fit — 8 points:** HLTH Europe, Dutch Pavilion, Amsterdam/June timing or meeting route.
- **Remote deliverability — 9 points:** can Truviq realistically assess and support remotely without needing on-site clinical operations?
- **Penalties:** missing enrichment, horizon/early-stage signal, no buyer route, no workflow owner, generic ecosystem-only signal.

Grade bands:
- **A: 85–100** — meeting target now.
- **B: 75–84** — qualify after one evidence gap is closed.
- **C: 60–74** — nurture/monitor.
- **D: below 60** — do not spend Sales-People call capacity yet.

Top current scored leads:

### TRU-HL-DE4622 — Nederlandse ziekenhuizen met SEH / NEED-context — A (97/100)
- Account type: Direct buyer
- Why fit: NL: SEH-doorlooptijden en kwaliteitsverschillen zijn procesintensief; Camunda-fit rond triage, diagnostiek, bedmanagement, escalaties, overdracht en kwaliteitsregistratie.
- Programme/procurement clue: NEED emergency-care quality registry; A&E throughput and quality-improvement programmes.
- Next evidence gap: Segment/prospect cluster; prioriteer later UMCs/topklinische ziekenhuizen met zichtbare digitalisering.
- Source: https://www.zorgvisie.nl/sehs-laten-gigantische-verschillen-zien-in-kwaliteit/

### TRU-HL-9425B2 — VVT reablement providers / wijkverpleging networks — A (97/100)
- Account type: Direct buyer
- Why fit: NL: underused reablement funding/space suggests implementation pain across intake, goals, multidisciplinary coordination, Wmo/wijkverpleging handoffs, monitoring and outcome reporting.
- Programme/procurement clue: Reablement implementation under AZWA/IZA elderly-care and district-nursing transformation; funding-space utilisation agenda.
- Next evidence gap: Funding may be a contracting/behaviour issue more than IT; validate operational bottlenecks at named VVT organisations.
- Source: https://www.zorgvisie.nl/nza-ruimte-voor-reablement-wordt-onvoldoende-benut/

### TRU-HL-9BE508 — Zorg bij jou / Santeon hybrid-care cooperative — A (97/100)
- Account type: Direct buyer
- Why fit: NL: large hybrid-care operating platform with 16 connected hospitals, 33,000+ patients, medical service centre and expanding care pathways; strong workflow-orchestration hypothesis around onboarding hospitals/pathways, monitoring exceptions, escalation to treating teams, information-security controls and outcome/reporting evidence.
- Programme/procurement clue: Zorg bij jou hybrid-care platform; connected-hospital rollout; AVL oncology hybrid pathway; NEN 7510 information-security continuous improvement.
- Next evidence gap: Public routing/governance contacts only: Zorg bij jou article names Siebren van der Kooij as director; AVL article names Anne den Hartog and Tjerk Maas as interviewees around AVL launch. Validate operational/budget owner before outreach; disqualify if workflow scope is fully owned inside existing platform/app supplier.
- Source: https://www.zorgbijjou.nl/zorgprofessionals

### TRU-HL-A29C8E — VZVZ / ZORG-ID ecosystem — A (92/100)
- Account type: Ecosystem influencer
- Why fit: NL: official ecosystem around ZORG-ID and UZI transition; strong process-governance angle for credential migration, role validation, exceptions and auditability.
- Programme/procurement clue: ZORG-ID Mobile / Smartcard migration; UZI transition; professional access modernisation.
- Next evidence gap: Likely ecosystem/authority account; direct commercial route and procurement responsibility need validation.
- Source: https://www.vzvz.nl/veelgestelde-vragen/kan-ik-een-zorg-id-smartcard-gebruiken-als-vervanging-voor-mijn-uzi

### TRU-HL-B39B6A — VZVZ 2026 roadmap / national exchange-services ecosystem — A (92/100)
- Account type: Ecosystem influencer
- Why fit: VZVZ year plan emphasizes operational excellence, reliable releases, ADG/EHDS, NVS and Wegiz; strong ecosystem angle for process standardisation, release coordination and governance workflows.
- Programme/procurement clue: VZVZ 2026 year plan: operational excellence, EHDS/ADG, NVS, Wegiz, reliable releases.
- Next evidence gap: Likely partner/ecosystem route; avoid assuming VZVZ is direct enterprise buyer without procurement evidence.
- Source: https://www.vzvz.nl/nieuws/vzvz-presenteert-jaarplan-2026-verbinding

### TRU-HL-43306E — Zilveren Kruis / digitale-triage contractering — A (92/100)
- Account type: Direct buyer
- Why fit: NL: payer-driven triage incentives create workflow governance needs around controlled rollout, exceptions, monitoring, accountability and provider coordination.
- Programme/procurement clue: Digital triage scale-up in GP care; AZWA breakthrough-funding model-plan candidate.
- Next evidence gap: Validate whether decisions sit with insurer, HIS/triage vendors or primary-care cooperatives; avoid assuming direct Camunda budget.
- Source: https://www.icthealth.nl/nieuws/financiele-druk-op-digitale-triage-zet-huisartsen-klem

### TRU-HL-CA4AE6 — Zorgverzekeraars Nederland / grote zorgverzekeraars — A (92/100)
- Account type: Direct buyer
- Why fit: NL: financierings- en beoordelingsroute voor uniforme transformatie/modelplannen kan proceslaag nodig hebben voor intake, besluitvorming, monitoring en audit; concrete kopers waarschijnlijk individuele verzekeraars of ZN-programmateams
- Programme/procurement clue: AZWA/IZA doorbraakmiddelen; modelplannen for scaling AI, digital/hybrid care and medical technology; waiting-list and transformation-plan agenda.
- Next evidence gap: Hypothese; officiële AZWA-regeling, eigenaren en procurement route valideren.
- Source: https://www.zorgvisie.nl/zo-kregen-zorgverzekeraars-toch-600-miljoen-aan-azwa-geld-te-verdelen/

### TRU-HL-A8D790 — AZWA chain-approach regional governance route — A (91/100)
- Account type: Regulatory/tender monitor
- Why fit: Turning chain approaches into basic functionalities creates multi-party workflow needs around eligibility, referral, handoffs, funding evidence, outcome reporting and governance.
- Programme/procurement clue: AZWA chain approaches; fall prevention; social referral; dementia chain approach; health-social-domain cooperation.
- Next evidence gap: TRU hypothesis; disconfirm if implementation remains policy/contracting without cross-system operational workflow.
- Source: https://www.zorgakkoorden.nl/actueel/nieuws/azwa-ketenaanpak-doorontwikkelen-tot-basisfunctionaliteit/

### TRU-HL-04CABC — Acute-care regions / MKA-ambulance-SEH-HAP networks — A (91/100)
- Account type: Regulatory/tender monitor
- Why fit: Updated acute-care standards point to time-critical handoff workflows across dispatch, ambulance, ED/SEH, GP/HAP and hospitals; orchestration/process-mining fit around exceptions, status and audit trails.
- Programme/procurement clue: Nictiz Acute Care information standard v2.2.0; emergency summary and care-report exchange.
- Next evidence gap: Segment prospect; identify named acute-care regions and hospitals before outreach.
- Source: https://www.nictiz.nl/nieuws/spoedsamenvatting-en-rapportage-verleende-zorg-vernieuwd/

### TRU-HL-CAC475 — Dezi / professional identity implementation ecosystem — A (91/100)
- Account type: Regulatory/tender monitor
- Why fit: Dezi migration away from UZI-pass limitations creates process-governance needs around professional onboarding, role/employer changes, approved login methods, supplier readiness and auditable medical-data access.
- Programme/procurement clue: Dezi professional login system; UZI-pass replacement/modernisation; healthcare professional authentication guidance.
- Next evidence gap: Official context; validate Dezi timeline, VWS/Nictiz/VZVZ roles and whether Camunda adds value beyond identity-provider tooling. Disqualify if workflow is limited to login technology with no cross-system exception/case handling.
- Source: https://www.nictiz.nl/nieuws/handreiking-dezi/

### TRU-HL-2F8462 — Dutch health CISO/CIO resilience segment — A (91/100)
- Account type: Regulatory/tender monitor
- Why fit: Major health hacks and political attention strengthen the case for incident-ready fallback workflows, breach communications, recovery sequencing and accountability across primary care systems.
- Programme/procurement clue: Cyber resilience / care-continuity programmes; incident-ready fallback workflows.
- Next evidence gap: Context signal; avoid treating as a named account until specific cyber-resilience programmes/procurements are found.
- Source: https://www.zorgvisie.nl/grote-zorg-hacks-schudden-politiek-wakker/

### TRU-HL-A39846 — Health-IT suppliers and provider release-governance leads — A (91/100)
- Account type: Regulatory/tender monitor
- Why fit: National release policy consultation points to coordination pain around standards release planning, impact assessment, supplier/provider readiness, exceptions and migration auditability.
- Programme/procurement clue: Nationaal Releasebeleid for health information standards.
- Next evidence gap: TRU hypothesis; validate whether Nictiz/national programmes, EHR vendors or large providers own the process budget.
- Source: https://www.nictiz.nl/nieuws/consultatie-nationaal-releasebeleid-van-start/


## 4. CRM-like, AI-enabled real-time exchange method

Recommended operating model: **Account Intelligence Hub**.

### Core principle

Sales-People, Truviq and Hermes/AI should not exchange long email summaries. They should exchange structured account events. Every call, email, website signal, tender signal and event meeting becomes an update to the same account record.

### Minimum CRM schema

For every account/lead:
- Lead ID: stable ID, e.g. `TRU-HL-A1B2C3`.
- Account name.
- Account type: direct buyer, ecosystem influencer, implementation partner, regulatory/tender monitor.
- Segment.
- Programme or named route.
- Current systems or standards.
- Buyer personas: programme owner, budget owner, implementation owner, procurement owner.
- Meeting status: target, called, routed, accepted, declined, nurture, disqualified.
- HLTH attendance: yes, no, unknown.
- Next action owner and date.
- Evidence links.
- Disqualifier.
- Score and score breakdown.
- AI summary: 5-bullet brief for the next caller/meeting.

### Automation architecture

1. **Source capture**
   - Sales-People call form.
   - Truviq manual notes.
   - Event-platform exports if available.
   - Public sources: official websites, tenders, programme pages, event pages.
   - Email/calendar meeting status.

2. **Ingestion and deduplication**
   - Store every input as an immutable note/event.
   - AI extracts account, person, role, programme, stack, timing, next action and disqualifier.
   - Deduplicate by organisation domain, KvK/name variant, LinkedIn/company URL, source URL and lead ID.

3. **Scoring engine**
   - Recompute score after every new event.
   - Show exactly which dimensions changed.
   - Trigger alerts only for meaningful movement: e.g. score +10, meeting accepted, buyer owner identified, tender published, disqualifier found.

4. **AI copilot layer**
   - Creates caller brief.
   - Suggests next question.
   - Flags unsupported claims.
   - Explains abbreviations.
   - Generates follow-up email after call.
   - Prepares HLTH meeting agenda.
   - Creates 4–6 week proof-of-value outline only after sponsor/workflow/procurement route are known.

5. **Exchange and governance**
   - Sales-People sees only call-needed fields and opt-out status.
   - Truviq sees full technical and commercial enrichment.
   - Every AI-generated field is labelled: source-backed, inferred, or needs validation.
   - Do not store patient data.
   - Use consent/opt-out logging for outreach.

### Practical tooling options

- Fastest: Airtable or HubSpot plus Make/Zapier/n8n plus an AI extraction webhook.
- More controlled: Git-backed JSON/CSV source of truth plus a GitHub Pages dashboard and Hermes cron enrichment.
- Best long-term: CRM system as the system of record, event stream/webhooks for changes, vector search over source notes, and a scored account dashboard.

Recommended for now: **hybrid file-first + CRM-ready export**. Keep `/home/hermes/work/truviq/markt-health/data/lead_scoring.csv` as the auditable source while piloting. Export accepted leads to the chosen CRM after the scoring mechanism stabilises.

## 5. What Truviq can realistically provide remotely

Realistic remote offers:
- 20-minute workflow-fit meeting.
- 60–90 minute remote discovery workshop.
- Current-state workflow map from interviews and screenshots/documents.
- Cross-system exception map.
- Buyer/persona/procurement route validation.
- BPMN/DMN model draft.
- FHIR/API integration hypothesis.
- AI-governance workflow design.
- 4–6 week proof-of-value using synthetic or anonymised data.
- Remote implementation support for Camunda/Pega/ServiceNow workflows, connectors and dashboards.

Remote limits:
- Cannot credibly promise clinical workflow change without provider-side owner participation.
- Cannot validate patient-safety impact remotely without clinical governance input.
- Cannot access production health data unless legal, security and procurement conditions are met.
- Cannot replace EHR/ECD vendor scope if the workflow is fully inside the incumbent system.
- Cannot sell “AI” generically in Dutch health; must tie it to a controlled process, audit trail and measurable value.

Best first paid offer:
- **4–6 week workflow discovery / proof-of-value sprint**.
- Inputs: one named healthcare workflow, one owner, current systems, pain metric, compliance constraint.
- Outputs: process model, exception map, integration options, governance controls, benefit case, implementation estimate.
- Success metric: a decision-ready case, not a finished enterprise platform.

## 6. Abbreviation glossary

- **AI:** Artificial Intelligence — software that performs tasks normally associated with human reasoning, prediction, generation or classification.
- **Agentic AI:** AI that can plan and act through tools or workflows, under guardrails.
- **API:** Application Programming Interface — a structured way for systems to exchange data or trigger actions.
- **AVG / GDPR:** Dutch/EU privacy law. AVG is the Dutch term; GDPR is the English abbreviation.
- **AZWA:** Aanvullend Zorg- en Welzijnsakkoord — Dutch additional health and welfare agreement; relevant here as a transformation-funding route.
- **BPM:** Business Process Management — managing and improving end-to-end business processes.
- **BPMN:** Business Process Model and Notation — a standard diagram language for workflows.
- **Camunda:** Process automation/orchestration platform using BPMN/DMN patterns.
- **CIO:** Chief Information Officer — senior IT/digital executive.
- **CISO:** Chief Information Security Officer — senior cybersecurity executive.
- **CMIO:** Chief Medical Information Officer — clinician leader for medical information systems.
- **CNIO:** Chief Nursing Information Officer — nursing leader for digital/nursing information systems.
- **CRM:** Customer Relationship Management — system/process for tracking leads, accounts, calls, meetings and opportunities.
- **DMN:** Decision Model and Notation — standard for decision tables/rules.
- **DPA:** Digital Process Automation — automating digital business processes.
- **ECD:** Electronic Client Dossier — client record system, often used in long-term care.
- **EHDS:** European Health Data Space — EU regulation/framework for health-data access, sharing and secondary use.
- **EHR / EPD:** Electronic Health Record / Elektronisch Patiëntendossier — digital patient record.
- **FHIR:** Fast Healthcare Interoperability Resources — healthcare data exchange standard.
- **GGZ:** Geestelijke Gezondheidszorg — Dutch mental healthcare.
- **HAP:** Huisartsenpost — GP out-of-hours post.
- **HLTH Europe:** Healthcare innovation event in Amsterdam, used here as a meeting deadline and routing mechanism.
- **IAM:** Identity and Access Management — managing who can access which systems/data.
- **ICP:** Ideal Customer Profile — description of the best-fit customer type.
- **IDP:** Intelligent Document Processing — AI/automation for extracting and routing information from documents.
- **IZA:** Integraal Zorgakkoord — Dutch integrated care agreement.
- **LCNC:** Low-code/no-code — software development with visual tools and reduced hand coding.
- **LSP:** Landelijk Schakelpunt — Dutch national exchange infrastructure for healthcare data.
- **Mitz:** Dutch consent service for health-data sharing.
- **NEN 7510:** Dutch information-security standard for healthcare.
- **Nictiz:** Dutch competence centre for digital information exchange in healthcare.
- **Nuts:** Decentralised healthcare data-sharing trust/infrastructure initiative.
- **Pega:** Enterprise low-code/business process platform.
- **PZP:** Proactieve Zorgplanning — proactive care planning, including treatment preferences/limits.
- **RAI Amsterdam:** Convention centre in Amsterdam where HLTH Europe takes place.
- **RAV:** Regionale Ambulancevoorziening — regional ambulance service.
- **ROAZ:** Regionaal Overleg Acute Zorgketen — regional acute-care network/coordination body.
- **RPA:** Robotic Process Automation — automation of repetitive computer tasks.
- **SEH:** Spoedeisende Hulp — emergency department.
- **SMART-on-FHIR:** Healthcare app authorisation pattern combining SMART app launch/security with FHIR data standards.
- **TFHC:** Task Force Health Care — Dutch health-sector network/organisation involved in international health events.
- **UZI:** Dutch healthcare professional identification/register mechanism.
- **VVT:** Verpleging, Verzorging en Thuiszorg — nursing, elderly care and home care.
- **VZVZ:** Vereniging van Zorgaanbieders voor Zorgcommunicatie — organisation behind several Dutch healthcare exchange services.
- **ZN:** Zorgverzekeraars Nederland — Dutch association of health insurers.

## 7. Next implementation steps

1. Convert the website changes above into a one-page health landing-page draft.
2. Give Sales-People only A/B leads and the meeting-focused script, not the technical positioning deck.
3. Use the lead scoring CSV as the first CRM import.
4. Add call-note fields and AI extraction before the first outreach batch.
5. Run a 10-account pilot before scaling.
