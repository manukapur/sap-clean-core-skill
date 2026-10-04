# SAP Clean Core Skill for Claude

An [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that turns Claude into a clean core advisor for SAP S/4HANA: extensibility decisions, A/B/C/D level classification, remediation planning and governance — grounded in SAP's own published guidance rather than general model knowledge.

**Based on:** SAP Note 3578329 v21 (05.08.2026) and the SAP Clean Core Extensibility White Paper.

## What it does

- **Classifies frameworks and technologies** against SAP Note 3578329 — e.g. "Is SEGW OData clean core?" → Level B, upgrade stable, not public cloud ready, alternative: RAP. Covers ~90 frameworks: exits, BAdIs, BTEs, enhancements, BOPF, BOR, BRF+, output technologies, UI technologies, ALE/IDoc, AIF and more.
- **Applies SAP's interpretation rules correctly**: extensibility-only scope, technology vs. individual-object ratings, conditional A/B, hidden C/D usage, single-implementation techniques.
- **Guides extensibility decisions**: configuration → key user → released BAdI → RAP → BTP side-by-side.
- **Explains release contracts** C0 (Extend), C1 (Use System-Internally), C2 (Use as Remote API), C3, C4.
- **Supports remediation and governance**: three-horizon roadmap, ARB/SSB, ATC enforcement, wrapper strategy, Boy Scout Principle, KPIs (Technical Debt Score, Clean Core Share).
- **Adapts to the landscape**: Public Edition (Level A only), Private Cloud/RISE and on-premise.

## Structure

```
sap-clean-core/
├── SKILL.md                                         # Core guidance and decision framework
└── references/
    ├── note-3578329-framework-classification.md     # Full framework/technology level table (v21)
    └── whitepaper-advanced.md                       # Governance, AEM, SSB, maturity model, scenarios
```

## Installation

**Claude.ai** — Download `sap-clean-core.skill` from [Releases](../../releases) (or zip the `sap-clean-core` folder) and upload it under *Settings → Capabilities → Skills*.

**Claude Code** — Copy the `sap-clean-core` folder into `~/.claude/skills/` (personal) or `.claude/skills/` in your project.

**Claude API** — Upload via the Skills API; see the [Anthropic docs](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview).

## Example prompts

- "Is a BTE implementation clean core? What should I replace it with?"
- "We have 400 implicit enhancements in our ECC — build me a remediation roadmap for a RISE move."
- "Classify these Z-objects A/B/C/D: SAP Query reports, BDC uploads to VA01, a SmartForm invoice."
- "Our partner wants to call a standard BAPI from their system. What clean core level is that?" (answer: the level concept doesn't apply — it's the integration dimension)

## Keeping it current

SAP updates Note 3578329 regularly. When a new version is released, update `references/note-3578329-framework-classification.md` and the version references in `SKILL.md`. Pull requests welcome — please cite the note version.

## Disclaimer

Community-maintained; not an official SAP product and not affiliated with or endorsed by SAP SE. Framework ratings are transcribed from SAP Note 3578329 v21; SAP's current note always takes precedence. Benchmarks and KPI targets are practitioner guidance. SAP, S/4HANA, ABAP and BTP are trademarks of SAP SE.

## Author

**Manu** — Senior SAP Technical & Solution Architect, [Markks Ltd](https://markks.co.uk)

## Licence

[MIT](LICENSE)
