---
name: sap-clean-core
description: >
  Use for ANY SAP Clean Core question — extensibility decisions, A/B/C/D level classification,
  C0-C4 release contracts, remediation roadmaps, governance (ARB, SSB, TDS/CCS KPIs, Boy Scout
  Principle), ATC findings, ABAP Cloud, RAP vs Key User vs BAdI vs BTP side-by-side, OData vs
  RFC, Business Events, upgrade readiness, RISE, Cloud ALM custom code analytics. Also trigger
  whenever the user asks if a specific framework or technology is clean core or what level it
  gets — SEGW OData, BOPF, BOR, BRF+, BTE, SMOD/CMOD exits, classic BAdIs, user exits/VOFM,
  enhancement points, implicit enhancements, appends, SAP Query, logical databases, BDC,
  SmartForms, SAPscript, Adobe Forms, Web Dynpro, FPM, ALV, WebClient UI, PPF, workflow,
  ALE/IDoc, AIF, Screen Personas — or mentions SAP Note 3578329. Use even for quick checks like
  "can I use this function module?"
---

# SAP Clean Core Skill

A comprehensive reference for SAP Clean Core architecture, extensibility, governance, and remediation — aligned to the official SAP Clean Core Extensibility White Paper, the A-D Extensibility Level model, and **SAP Note 3578329 v21 (05.08.2026)** for framework/technology classification.

## How to Use This Skill

- **Establish the landscape first** when it changes the answer: S/4HANA Cloud Public Edition, Private Cloud Edition (RISE), or on-premise; the release (e.g. 2023, 2025); and greenfield vs. brownfield. Public Edition allows only Level A; Private Cloud and on-premise can tolerate governed B/C. If the user hasn't said and it matters, ask once — otherwise state the assumption.
- **Framework/technology level questions** → answer from `references/note-3578329-framework-classification.md` and cite the note version.
- **Strategy, governance, remediation, roadmap questions** → use the sections below; load `references/whitepaper-advanced.md` for deep dives.
- **Individual API/object questions** ("is function module X released?") → check the release state in the Cloudification Repository (github.com/SAP/abap-atc-cr-cv-s4hc) or ADT's API State; don't guess from the object name.
- **Recency**: SAP updates its clean core notes frequently. If web access is available and the user needs a definitive answer, check whether a newer version of Note 3578329 exists than the one this skill is based on.

> **Author**: Manu, [Markks Ltd](https://markks.co.uk). Released under the MIT License. Source and updates: https://github.com/manukapur/sap-clean-core-skill
>
> **Disclaimer**: Community-maintained skill, not an official SAP product; not affiliated with or endorsed by SAP SE. SAP, S/4HANA, ABAP and BTP are trademarks of SAP SE. Framework ratings are transcribed from SAP Note 3578329 v21 (05.08.2026); SAP's current note always takes precedence. Benchmarks and KPI targets below are practitioner guidance, not SAP-published figures.

---

## Quick Decision Framework — Use This First

For any extension or custom code question, apply in order:

| Step | Question | If YES → Stop |
|------|----------|---------------|
| 1 | Can standard SAP configuration handle this? | Configure it |
| 2 | Can Key User Extensibility handle this? (Custom Fields & Logic app) | Use Key User |
| 3 | Is there a released BAdI / enhancement spot? | Implement ABAP Cloud BAdI |
| 4 | Does this need a new business object or API? | Build with RAP (ABAP Cloud) |
| 5 | Complex UI, cross-system logic, or AI/ML? | Side-by-side BTP extension |
| 6 | None of the above? | Challenge requirement. File SAP influence request |

**Golden Rule**: Never modify SAP standard objects. Only consume C1 (released) APIs.

---

## The A-D Extensibility Levels (August 2025)

| Level | Label | API Type | Upgrade Risk | New Dev? | Governance |
|-------|-------|----------|-------------|----------|------------|
| **A** | Gold Standard | Released C1 APIs only (ABAP Cloud / BTP) | None — SAP guarantee | **Mandatory for all new dev** | No special approval |
| **B** | Conditionally OK | Classic APIs/frameworks — upgrade stable but not public cloud ready (customer exits SMOD/CMOD, classic BAdIs, BTEs, SEGW OData, BAPI framework, BOPF, BOR, SmartForms/SAPscript, Web Dynpro, FPM, ALV display, classic/flexible workflow) | Low-Medium | Acceptable transitionally | ARB awareness; log in inventory |
| **C** | Conditionally Clean | SAP-internal (unreleased) objects; frameworks whose purpose equals arbitrary table access (SAP Query, Logical Databases); BDC sessions recorded on SAP transactions; ALV edit mode | High | Avoid — exception only | Time-boxed remediation plan required |
| **D** | Not Clean Core | Modifications, implicit enhancements, explicit enhancement points/sections, obsolete APIs (e.g. ODP RFC APIs), direct writes to SAP tables | Critical | **NEVER** | Emergency remediation — zero tolerance |

### How SAP says to apply the levels (Note 3578329)

- **Levels rate the extensibility dimension only.** In scope: custom/partner code calling a framework API, implementing SAP BAdIs/enhancements, creating own objects (own CDS view, own Fiori app). **Out of scope**: an external system calling a standard SAP OData/SOAP/IDoc/BAPI, or using standard functions (archiving, ILM, batch jobs) with no customer extension. Don't assign A-D to those — integration is assessed under SAP Note 3690029.
- **Technology vs. individual object.** Building your own CDS view = A. Consuming a specific SAP CDS view or BAPI = whatever *that object's* release state gives it (A/B/C/D).
- **Conditional "A/B".** Same framework, two API surfaces: C1-released APIs = A; classic APIs = B. Applies to Application Jobs, Application Log, Number Ranges, Change Documents, BRF+, UoM, Currency, Factory Calendar, ADK, ILM, AIF, SOAP proxies, Adobe/Forms Processing, Cloud Printing, BCS, Email Templates.
- **Every framework has a hidden C/D.** Calling its SAP-internal or obsolete function modules/classes drops usage to C or D even when the framework row says B.
- **Single-implementation techniques aren't penalised.** One-active-implementation exits/BAdIs keep their level; managing clashes with add-ons is the customer's job.

**For any "what level is framework X?" question, read `references/note-3578329-framework-classification.md`** — it holds the full v21 table with components, public-cloud alternatives and related notes. Quote SAP's rating rather than reasoning it out, and use the English note (v21 excluded parts from machine translation because translated levels were wrong).

**ATC mapping**: Level D = Priority 1 (BLOCKS transport) | Level C = Priority 2 (Warning, can block) | Level B = Priority 3 (Informational) | Level A = No finding

---

## Release Contracts (C0–C4)

Release contracts are what an object is *released for*. "Not released" is a release **state**, not a contract.

| Contract | Meaning | Typical use |
|----------|---------|-------------|
| **C0** — Extend | Stable extension point (e.g. extension include of a DDIC structure/table, extensible CDS view, released BAdI enhancement spot) | Field extensions, appends in ABAP Cloud |
| **C1** — Use System-Internally | Released for consumption within the same system by ABAP Cloud / key user code. Stable across upgrades. | Calling APIs, reading released CDS views |
| **C2** — Use as Remote API | Released for consumption from outside the system (OData/SOAP/RFC) | Integration, side-by-side |
| **C3** — Manage Configuration Content | Released for managing config content | Configuration tooling |
| **C4** — Use in ABAP-Managed Database Procedures | Released for use in AMDP | AMDP |

**Not released** (no contract) = SAP-internal → Level C at best, blocked by the ABAP Cloud language version. Deprecated objects have a named successor — migrate.

---

## The Four Extensibility Layers

### Layer 1: Key User Extensibility (No code required)
- Custom fields on standard screens and reports
- Custom business objects (lightweight entities)
- Custom logic with SAP formula builder
- Custom analytical queries and workflows
- **Rule**: If it can be done here, it MUST be done here. Never use developer extensibility when key user tools suffice.

### Layer 2: ABAP Cloud Development (On-stack, C1 only)
- **BAdIs**: Primary extension hook. SAP pre-defines extension points; you implement the interface. Upgrade-safe.
- **RAP (RESTful Application Programming Model)**: Modern way to build custom business objects and expose as OData. Use RAP for any new custom BO.
- **CDS Views**: For data access — only released CDS views, never direct table SELECTs on unreleased SAP tables.
- **Restrictions**: No unreleased SAP API calls, no obsolete ABAP statements, must activate ABAP Cloud profile in ADT.

### Layer 3: Classic ABAP (Legacy — avoid, plan to retire)
- Z-programs, exits, legacy BAdIs
- Classify as Level B (classic exits/BAdIs) or Level C/D (if unreleased SAP objects, enhancement points/implicit enhancements, or modifications)
- Target: refactor to ABAP Cloud or retire

### Layer 4: Side-by-Side BTP (For complex, cross-system, AI scenarios)
- CAP (Cloud Application Programming model), SAP Build, Fiori custom apps
- Always connects back to S/4HANA via released OData APIs or Business Events — never direct RFC
- Use for: complex UI/UX, cross-system orchestration, AI/ML, high-volume analytics

---

## Integration Patterns

**Core Rule**: Every integration must go through a released, stable, versioned API. Never call unreleased RFCs directly.

**Scope reminder**: Note 3578329's A-D levels do *not* rate an external system calling a standard SAP API — that falls under the integration dimension (SAP Note 3690029). The levels *do* apply to what you build in the core to provide the interface: a custom OData service built in SEGW = B; built via Service Definition/Binding (RAP) = A. ODP Framework RFC APIs = D (Note 3255746).

| Scenario | Pattern | Anti-Pattern |
|----------|---------|-------------|
| External system creates SAP doc real-time | Released OData POST API via CPI | Direct RFC call |
| React when SAP data changes | Business Event subscription via Event Mesh | Polling / QRFC |
| High-volume overnight sync | Released IDoc or bulk OData | Direct SQL extraction |
| Master data distribution | MDG + Business Events | Manual extracts |
| Complex cross-system orchestration | BTP CAP + Integration Suite | Custom middleware with unreleased FMs |

**SAP Integration Suite components**: CPI (integration flows), API Management, Event Mesh, Integration Advisor (B2B/EDI), Open Connectors (170+ pre-built).

---

## Upgrade & Operations

### Why Clean Core Enables Continuous Innovation
- Heavily modified cores typically need many months of upgrade preparation; clean cores need weeks (practitioner experience, varies widely)
- Public Edition: SAP-managed, mandatory upgrades several times a year — only Level A extensions survive. Private Cloud / on-premise: customer-scheduled releases and feature pack stacks — check SAP's current release strategy for exact cadence and maintenance windows
- Released (C1/C2) objects: SAP backward compatibility guaranteed | Unreleased objects: may break with no SAP obligation

### ABAP Test Cockpit (ATC) — Enforcement Gate
ATC runs static code analysis before transport. Recommended setup in mature governance: **ATC critical findings (Priority 1 / Level D) block transports to production** (configure this in your ATC variant and transport release checks).

Checks include: unreleased API usage, obsolete ABAP statements, direct SELECT on non-released tables, hard-coded system dependencies.

**Key SAP Notes**:
- Note 3578329 (v21, 05.08.2026): Frameworks, technologies & development patterns — A/B/C/D level per framework. Full table in `references/note-3578329-framework-classification.md`
- Note 3690029: Integration technologies & frameworks in context of clean core *integration*
- Note 3632977: DDIC extensions (appends) — A/B/C/D depending on technique
- Note 3589866: SD/LE user exits & VOFM — B/D
- Note 3641991: Business Transaction Events — B
- Note 3478579: Business Object Repository — B
- Note 3591718: WebClient UI — B
- Note 3565942: ATC Check Assignment — which ATC checks map to each level
- API release states: Cloudification Repository at github.com/SAP/abap-atc-cr-cv-s4hc

**S_ABPLNGVS**: Authorization object to restrict developers to 'ABAP for Cloud Development' language version — a powerful preventive governance control.

### Tools Reference

| Tool | Phase | Purpose |
|------|-------|---------|
| SAP Readiness Check | Assessment | One-time scan for S/4HANA upgrade readiness |
| Cloud ALM Custom Code Analytics | Assessment + Ongoing | Z-object A-D levels, ATC findings, dead code, upgrade risk |
| ABAP Test Cockpit (ATC) | Development + CI/CD | Blocks non-Clean Core from transport |
| SAP Business Accelerator Hub (api.sap.com) | Development | Catalog of all released OData APIs, Events, IDocs |
| SAP Integration Suite (BTP) | Integration | CPI, API Management, Event Mesh |
| SAP MDG | Data Governance | Master data governance hub |
| SAP Cloud ALM | Operations | Monitoring, alerting, test management, lifecycle |
| SCMON / SUSG | Ongoing | Identify Z-objects with zero runtime usage (dead code) |
| SMODILOG | Assessment | Log of SAP standard code modifications |
| SAP for Me — Clean Core Dashboard | Ongoing | Executive dashboard for RISE PCE systems |

---

## Governance & Strategy

### KPI Formulas

**Technical Debt Score (TDS)**:
```
TDS = (P1 findings × 10) + (P2 findings × 5) + (P3 findings × 1)
Target for upgrade readiness: TDS < 100
```

**Clean Core Share (CCS)**:
```
CCS = (Level A + Level B active Z-objects) ÷ (Total active Z-objects) × 100%
Target: > 80% for upgrade readiness; > 90% for mature organizations
```

### Target KPIs

| KPI | Target |
|-----|--------|
| % Z-objects using only C1 APIs | > 90% |
| ATC critical findings in production | 0 |
| Unreleased RFC calls in integration | 0 |
| % integrations using released OData/Events | > 85% |
| Dead code ratio | < 5% |
| Upgrade preparation time | < 30 days |
| ARB exception rate | < 20% |

### Three-Horizon Roadmap

| Horizon | Timeline | Key Activities |
|---------|----------|----------------|
| H1: Stop the Bleeding | 0–6 months | Freeze new non-Clean Core dev. Enable ATC enforcement. Retire dead Z-code. Establish governance board. Train devs. |
| H2: Remediate | 6–18 months | Refactor high-risk Z to ABAP Cloud. Replace RFC integrations. Implement MDG. Move complex to BTP. |
| H3: Sustain | 18–36 months | Clean Core KPIs as business reporting. Automated quality gates in CI/CD. Quarterly ARB reviews. |

### Three Lines of Defense

| Line | Role | Responsibilities |
|------|------|-----------------|
| Line 1: Dev Teams | Day-to-day | Follow Clean Core by design. ATC self-checks. |
| Line 2: Architecture Review Board (ARB) | Quality gate | Approve extensibility approach. Review CC score monthly. Exception management. |
| Line 3: Executive Sponsor | Strategic | Clean Core as KPI. Budget for remediation. Business case owner. |

---

## Advanced Topics (Whitepaper Deep-Dive)

For detailed coverage of these topics, read: `references/whitepaper-advanced.md`

- **Two-Layer IT Architecture** (S/4HANA stable core + BTP innovation layer)
- **SAP Application Extension Methodology (AEM)** — 3-phase decision framework
- **SAP Build + Joule AI** — low-code/no-code + AI co-pilot
- **Cloudification Repository** — authoritative release-state catalog (released / not released / deprecated, with successors)
- **Stay Clean / Get Clean patterns** + **Boy Scout Principle**
- **Wrapper/Encapsulation strategy** for B/C remediation
- **Exemption Process** — formal exception governance with Quality Manager role
- **Process Category Stratification** (Standard / Enhanced / Custom / Innovative)
- **Governance Maturity Framework** (0–5 scale)
- **SAP LeanIX** (Enterprise Architecture) + **SAP Signavio** (Process Intelligence)
- **Solution Standardization Board (SSB)** vs. Architecture Review Board (ARB)
- **Partner Add-Ons + SAP ICC Certification**

---

## Brownfield vs. Greenfield

| Dimension | Greenfield | Brownfield |
|-----------|-----------|-----------|
| Priority | Enforce Clean Core from day 1 | Assess, classify, remediate |
| Quick win | 100% ABAP Cloud from start | Retire dead code (fast, low risk) |
| Biggest risk | Pressure to deviate 'just this once' | Running out of remediation budget |
| Timeline | Immediate | 2–4 year program |

**The Brownfield Trap**: 'Lift and shift' moves the problem, not solves it. True Clean Core brownfield requires courage to say: "We will not migrate this until it is clean."

---

## Indicative Brownfield Benchmarks

Practitioner rules of thumb, not SAP-published statistics. Present them as indicative and replace with the customer's own measurements (SCMON/SUSG, ATC, Cloud ALM) as soon as available.

- Typical S/4HANA brownfield: thousands of Z-objects (2,000–5,000+ is common)
- A large share is dead code — often 30–40% never executed
- A majority of the remainder usually touches unreleased SAP APIs
- See the illustrative transformation scenario in `references/whitepaper-advanced.md` for a worked example of a remediation programme
