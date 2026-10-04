# SAP Clean Core Skill for Claude

[![Licence: MIT](https://img.shields.io/badge/licence-MIT-blue.svg)](LICENSE)
[![SAP Note 3578329](https://img.shields.io/badge/SAP%20Note%203578329-v21%20(05.08.2026)-0a6ed1)](https://me.sap.com/notes/3578329)
[![Latest release](https://img.shields.io/github/v/release/manukapur/sap-clean-core-skill)](https://github.com/manukapur/sap-clean-core-skill/releases/latest)

An [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that turns Claude into a clean core advisor for SAP S/4HANA: extensibility decisions, A/B/C/D level classification, remediation planning and governance — grounded in SAP's own published guidance rather than general model knowledge.

**Based on:** SAP Note 3578329 v21 (05.08.2026) and the SAP Clean Core Extensibility White Paper.

## Why use it

General-purpose AI gives plausible clean core answers. This skill gives SAP's answer.

| Question | Typical general AI answer | With this skill |
|---|---|---|
| A partner system calls a standard BAPI. Is that a clean core violation? | "BAPIs aren't released, so it's Level C" | The A–D level concept doesn't apply. Calling a standard remote API from another system belongs to the integration dimension (SAP Note 3690029) |
| What does the C0 release contract mean? | "Not released / SAP-internal" | C0 is the **Extend** contract (stable extension points). "Not released" is a release state with no contract |
| Are customer exits (SMOD/CMOD) acceptable? | "Level D for partners, B for customers" (outdated) | Level B for everyone since note v10/11; replace with kernel-based BAdIs |
| Is SAP Query OK for reporting? | "It's standard SAP, so yes" | Level C: equivalent to arbitrary table access. Use CDS views |

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

**Claude.ai** — Download `sap-clean-core.skill` from the [latest release](https://github.com/manukapur/sap-clean-core-skill/releases/latest) (or zip the `sap-clean-core` folder) and upload it under *Settings → Capabilities → Skills*.

**Claude Code** — Copy the `sap-clean-core` folder into `~/.claude/skills/` (personal) or `.claude/skills/` in your project.

**Claude API** — Upload via the Skills API; see the [Anthropic docs](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview).

## Example prompts

- "Is a BTE implementation clean core? What should I replace it with?"
- "We have 400 implicit enhancements in our ECC — build me a remediation roadmap for a RISE move."
- "Classify these Z-objects A/B/C/D: SAP Query reports, BDC uploads to VA01, a SmartForm invoice."
- "Our partner wants to call a standard BAPI from their system. What clean core level is that?" (answer: the level concept doesn't apply — it's the integration dimension)

## Keeping it current

SAP updates Note 3578329 regularly. When a new version is released, update `references/note-3578329-framework-classification.md`, the version references in `SKILL.md` and this README's badge, and add an entry to [CHANGELOG.md](CHANGELOG.md). Releases are tagged with the note version they track (e.g. `v1.0.0-note3578329-v21`).

Spotted a newer note version or a wrong rating? [Open an issue](https://github.com/manukapur/sap-clean-core-skill/issues) citing the note version, or send a pull request.

## Disclaimer

Community-maintained; not an official SAP product and not affiliated with or endorsed by SAP SE. Framework ratings are transcribed from SAP Note 3578329 v21; SAP's current note always takes precedence. Benchmarks and KPI targets are practitioner guidance. SAP, S/4HANA, ABAP and BTP are trademarks of SAP SE.

## Author

**Manu** — Senior SAP Technical & Solution Architect, [Markks Ltd](https://markks.co.uk)

## Licence

[MIT](LICENSE)
