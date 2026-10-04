# SAP Note 3578329 — Framework, Technology & Pattern Classification

**Source**: SAP Note 3578329 "Frameworks, Technologies and Development Patterns in Context of Clean Core Extensibility", **Version 21, released 05.08.2026**, component BC. Valid for S4CORE 108, 109, 110.

Load this file whenever the user asks whether a specific framework, technology, exit, enhancement type, UI technology, output technology or development pattern is clean core, or what level (A/B/C/D) it gets. The table is SAP's authoritative answer — quote the level and the "Public Cloud Ready alternative" from here rather than reasoning from first principles.

Column meanings: **Level** = clean core extensibility level. **Upg** = upgrade stable. **PC** = Public Cloud Ready (available in ABAP-based public cloud products such as S/4HANA Cloud Public Edition; renamed from "Cloud Ready" in v14). **Alternative** = SAP's Public Cloud Ready alternative.

---

## Interpretation Rules (read these before quoting a level)

1. **Technology vs. usage of individual SAP objects.** "CDS Views (Technology)" = A: building your own CDS view is fine. "Usage of individual SAP CDS views" = A/B/C/D depending on the release state of *that specific* view. Same split for BAPIs (framework = B; individual BAPI = B/C/D). Unless a row says "Usage of individual SAP…", the rating is for the framework/technology in general.

2. **Conditional A/B.** Where a framework has both C1-released APIs and classic APIs, using the released APIs = **A** (upgrade stable + cloud ready); using the classic APIs = **B** (upgrade stable only). That is what "A/B, conditional" means in the table — cloud readiness depends on which API surface you call.

3. **Hidden C/D on every framework.** Almost every framework also has SAP-internal (C) or obsolete (D) APIs and access paths. SAP does *not* list these as "…/C/D" in the table for readability, but they exist. A framework rated B can still be consumed at C or D if you call its internal function modules or classes.

4. **Levels apply ONLY to the extensibility dimension.** In scope: calling a framework's API from custom/partner code; implementing SAP enhancements/BAdIs or extending SAP objects; creating own objects (own CDS view, own Fiori app) related to a framework. **Out of scope** (level concept does not apply): calling a standard SAP remote API (OData, web service, IDoc, BAPI) *from another system*; using standard SAP functionality (archiving, ILM, batch jobs) with no customer extension involved. Integration is assessed separately — see SAP Note 3690029.

5. **Single-implementation techniques are not penalised.** Customer exits and single-use BAdIs allow only one active implementation; clashes (e.g. a 3rd-party add-on shipping its own implementation) can cause short dumps. This does **not** lower the clean core level — avoiding clashes is the customer's responsibility.

6. **Job frameworks rate the framework, not the job.** Application Jobs / SM37 ratings apply to using the scheduling framework in an extension context, not to every report that might be scheduled through it.

7. **Application-specific extensibility is decomposed.** E.g. "TM Extensibility" has no single rating; rate each underlying technology (BAdI, BOPF, BRF+, Adobe Forms, FPM, Fiori, Web Dynpro, PPF, enhancements) separately.

8. **Release state lookup**: https://github.com/SAP/abap-atc-cr-cv-s4hc (Cloudification Repository) — gives release state and classification of individual APIs.

---

## Level A — Upgrade stable and Public Cloud Ready

| Framework / Technology | Component | Notes |
|---|---|---|
| BAdI (Kernel based) — technology | BC-DWB-CEX | Preferred target for all exit/enhancement remediation |
| RAP / RAP Extensibility | BC-ESI-RAP-SRV | |
| OData via Service Definition / Service Binding | BC-ESI-RAP-SRV | |
| CDS Views (Technology) — own views | BC-DWB-DIC | |
| Extension Components Library (XCO) | BC-SRV-APS-EXT-XCO | |
| Manage Substitution and Validation Rules (Fiori) | FI-SL-VSR | |
| Fiori | CA-UI5 | |
| SAP Screen Personas for SAP GUI for HTML (WebGUI) | BC-PER | |
| SAPUI5 Key User Adaptation | CA-UI5-FL-RTA | |
| SAPUI5 Developer Adaptation (Adaptation Project, type "Cloud Ready") | CA-UI5-FL-ADP | |

## Level A/B — Conditional (A with released APIs, B with classic APIs)

| Framework / Technology | Component | PC alternative / detail |
|---|---|---|
| Application Jobs | BC-SRV-APS-APJ | Application Jobs |
| Factory Calendar | BC-SRV-ASF-CAL | Factory Calendar |
| Units of Measurement | BC-SRV-ASF-UOM | Unit of Measurement |
| Currency Handling | BC-SRV-BSF-CUR | Currency Conversion |
| Archive Development Kit | BC-CCM-ADK | Data Archiving |
| Information Lifecycle Management (ILM) | BC-ILM | Data Destruction |
| AIF (Application Interface Framework) | BC-SRV-AIF | SAP Note 3622023 |
| SOAP Web Service Framework — Proxy Generation | BC-DWB-PRX / BC-DWB-WS-ABA / BC-ESI-WS-ABA | SAP Note 3594071 |
| Email Templates | BC-SRV-COM-OM | A if the template's ABAP language version is ABAP Cloud |
| Business Communication Services | BC-SRV-COM | Output Management — Email Services |
| Forms Processing | BC-SRV-FP | Output Management — Print Forms |
| Adobe Forms | BC-SRV-FP-OM | Output Management — Print Forms |
| Cloud Printing | BC-SRV-PRN-OM | Output Management — Printing |
| Business Rules Framework (BRF+) | BC-SRV-BR | BRF+ released APIs |
| Change Documents | BC-SRV-ASF-CHD | Change Documents |
| Number Ranges | BC-SRV-NUM | Number Ranges |
| Application Log | BC-SRV-BAL | Application Log |

## Level B — Upgrade stable, NOT Public Cloud Ready

| Category | Framework / Technology | Component | PC alternative / detail |
|---|---|---|---|
| Application | Organizational Management | BC-BMT-OM | |
| Application | Radio Frequency Framework (EWM) | SCM-EWM-RF | |
| Application | Business Data Toolset (BDT) | CA-GTF-BDT | |
| Automation | Post-Processing Framework (PPF) | BC-SRV-GBT-PPF | |
| Automation | Classical Batch Jobs (SM37) | BC-CCM-BTC | |
| Automation | Technical Job Repository | BC-CCM-BTC-JR | |
| Automation | Classic Substitution & Validation (GGB0/GGB1) | FI-SL-VSR | Manage Substitution and Validation Rules (Fiori) |
| Automation | FI-CA Mass Activities | FI-CA | SAP Note 144461 |
| Customizing | Date Rules | BC-SRV-TIM-TR | |
| Data mgmt | Document Relationship Browser | BC-SRV-GBT-DRB | |
| Data mgmt | Table Analysis TAANA | BC-CCM-TAN | |
| Data mgmt | Archive Information System | BC-CCM-ADK-AS | |
| Data mgmt | Notes for Application Objects | BC-SRV-GBT-NTE | |
| Data mgmt | Generic Object Services (GOS) | BC-SRV-GBT-GOS | |
| Data mgmt | Online Text Repository (OTR) | BC-SRV-OTR | |
| Data mgmt | Business Document Service | BC-SRV-BDS | |
| Data mgmt | ArchiveLink | BC-SRV-ARL* | |
| Data mgmt | Knowledge Provider (KPro) | BC-SRV-KPR* | BTP Document Management Service |
| Documentation | Documentation Tools | BC-DOC-DTL | |
| Documentation | Knowledge Warehouse | KM-KW-* | |
| Enhancement | Customer Exits (SMOD/CMOD) | BC-DWB-CEX | BAdI (Kernel based). Check table MODSAP if unsure whether an exit is SMOD/CMOD type |
| Enhancement | BAdI (classic) | BC-DWB-CEX | BAdI (Kernel based) |
| Enhancement | Business Transaction Events (BTE) | CA-GTF-TS-BRHF | BAdI (Kernel based); SAP Note 3641991 |
| Enhancement | FI-CA Events / FQEVENTS | FI-CA | Specific BAdIs |
| Integration | ALE & IDoc | BC-MID-ALE | Process Integration Technologies |
| Integration | OData via SAP Gateway Service Builder (SEGW) | OPU-BSE-SB | RAP |
| Integration | BAPI Framework — technology | BC-MID-API | RAP |
| Other | Business Object Processing Framework (BOPF) | BC-ESI-BOF | |
| Other | Business Object Repository (BOR) | BC-DWB-TOO-BOB | RAP events, SAP Event Mesh; SAP Note 3478579 |
| Other | ABAP Shared (Memory) Objects | BC-DWB-TOO-SHM | |
| Output | Alert Management | BC-SRV-GBT-ALM | |
| Output | SAPscript | BC-SRV-SCR | Output Management — Print Forms |
| Output | SmartForms | BC-SRV-SSF | Output Management — Print Forms |
| Output | Spool | BC-CCM-PRN | Output Management — Printing |
| Output | NAST | SD-BF-OC | |
| Process flow | Classic Workflow | BC-BMT-WFM | SAP Build Process Automation (BTP) |
| Process flow | Flexible Workflow | BC-BMT-WFM | SAP Build Process Automation (BTP) |
| Reuse | SAPoffice | BC-SRV-COM | |
| Reuse | SAPphone | BC-SRV-COM-TEL | |
| Reuse | Object Link | BC-SRV-GBT-OBL | |
| Reuse | Appointment Calendar | BC-SRV-GBT-CAL | |
| Reuse | Case Management | BC-SRV-CM | |
| Reuse | Records Management | BC-SRV-RM | |
| Reuse | Geographical Enablement Framework | CA-EPT-GEF | Customizing-driven ABAP class enhancements; Fiori extension points; XCO-based spatial tables |
| UI | Standard Dialogs | BC-SRV-ASF-POP | Fiori/UI5 |
| UI | Dynamic Documents | BC-CI-DYD | |
| UI | ABAP Reports | BC-ABA-LA | Fiori/UI5 |
| UI | Transaction Variants (SHD0) | BC-ABA-TV | |
| UI | ALV — display functionality | BC-SRV-ALV | Fiori/UI5 |
| UI | Floor Plan Manager | BC-WD-CMP-FPM | Fiori Elements |
| UI | Web Dynpro | BC-WD-ABA | Fiori/UI5 |
| UI | WebClient UI | CA-WUI | Fiori/UI5; SAP Note 3591718 |
| UI | Pivot Browser Reporting Framework | PSM-FM-IS | |

## Mixed / conditional levels involving C or D

| Framework / Technology | Level | Upg | PC | Detail |
|---|---|---|---|---|
| Batch Input / BDC | **B/C** | conditional | no | BDC_* function modules (the framework) = B; the individual batch input sessions recorded on SAP transactions = C |
| DDIC Field Extensions (e.g. appends) | **A/B/C/D** | conditional | conditional | Alternative: Key User custom fields & logic, C0 extensibility in ABAP Cloud. See SAP Note 3632977 |
| SD/LE User Exits (form routines) & VOFM routines | **B/D** | conditional | no | Alternative: BAdI (Kernel based). See SAP Note 3589866. No "C" — there are no SAP-internal user exits |
| Usage of individual SAP BAPIs | **B/C/D** | conditional | no | Depends on the individual BAPI's release/classification; see its successor |
| Usage of individual SAP CDS views | **A/B/C/D** | conditional | conditional | Depends on the individual view's release state; see its successor |

## Level C — Upgrade stable framework, but usage is equivalent to direct table access

| Framework / Technology | Component | Alternative | Why C |
|---|---|---|---|
| SAP Query | BC-SRV-QUE | CDS Views | Framework is stable, but its main purpose is arbitrary table access — same disadvantages as direct table reads |
| Logical Databases | BC-DWB-TOO-LDB | CDS Views | Same reasoning as SAP Query |
| ALV Edit functionality | BC-SRV-ALV | Fiori/UI5 | Experimental, not intended for customer use (SAP Notes 551605, 695910). ALV *display* remains B |

## Level D — Not upgrade stable, not cloud ready

| Framework / Technology | Component | Alternative / detail |
|---|---|---|
| Explicit Enhancement Options excl. BAdIs (Enhancement Points & Sections) | BC-DWB-CEX | BAdI (Kernel based) |
| Implicit Enhancement Options | BC-DWB-CEX | BAdI (Kernel based). Includes: implicit options in ABAP source; enhancing FM parameter interfaces; enhancing components of global classes/interfaces |
| ODP Framework RFC APIs | BC-BW-ODP | SAP Note 3255746 |
| SAP Screen Personas for Web Dynpro ABAP / for SAP GUI for Windows | BC-PER | Sunset. SAP Notes 3239603, 2080071. Not to be confused with Personas for WebGUI (A) |

---

## Related SAP Notes

| Note | Topic |
|---|---|
| 3690029 | Integration Technologies and Frameworks in Context of Clean Core **Integration** (the integration-dimension counterpart to this note) |
| 3632977 | Clean Core Extensibility: DDIC Extensions |
| 3589866 | Clean Core: User Exits & VOFM in SD/LE |
| 3641991 | Business Transaction Events (BTE) |
| 3622023 | AIF |
| 3594071 | SOAP Web Service Proxy Generation |
| 3478579 | Business Object Repository in context of Clean Core |
| 3591718 | WebClient UI Framework in context of Clean Core |
| 3255746 | ODP Framework RFC APIs |
| 3406389 | Clean Core for SAP S/4HANA Utilities Cloud Private Edition |

## Changelog highlights relevant to past advice

- **v10/11**: Customer exits changed from "B for customers / D for partners" to **B in general**. Classic BAdIs added (B). User exit classification corrected from B/C to **B/D**.
- **v14**: "Cloud Ready" renamed "Public Cloud Ready".
- **v15/16**: BOR changed from B/C conditional to **B unconditionally**. Flexible Workflow added (B).
- **v17**: BDT (B), Batch Input (B/C), ODP RFC APIs (D), Email Templates can be A, Personas for WebDynpro/WinGUI (D).
- **v18**: SOAP proxy generation (A/B), Shared Memory Objects (B), Pivot Browser (B).
- **v19**: Geographical Enablement Framework (B), ALV Edit (C).
- **v20**: FI-CA Mass Activities (B), Logical Databases (C).
- **v21**: Parts of the note excluded from machine translation because translations garbled the levels — **always use the English version** of the note.
