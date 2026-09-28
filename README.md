# Intent-Based Developer Platform

A platform strategy case study: how a Developer Experience team moves an organization from manual infrastructure configuration to an **AI-agentic, intent-based** deployment model, without trading reliability for velocity, and without asking skeptical senior engineers to trust a black box.

Grounded in Yahoo as a real, public example of the setting: their Platforms org runs Cloud Runtime (Kubernetes), Security, and Messaging as the underlying capabilities a developer has to glue together to ship anything, per their public Sr. PM, Platforms job posting. Everything past that is my own analysis, not Yahoo's internal roadmap or proprietary information. I have no visibility into their actual architecture or metrics.

## The problem

Engineers write code fast and then lose days hand-gluing four disconnected platform capabilities (Provisioning, Security & Compliance, Observability, Traffic Management) to get a simple microservice into production. This is a 6-month plan to replace that manual "glue" with an **AI intent layer**: a developer describes what they're deploying, however they already have it (a one-line blurb, a POC repo, a legacy service to modernize), the agent shows back what it understood and composes it from pre-approved, human-authored modules, and a person still approves every change before it ships.

## What's here

| File | Contents |
|---|---|
| [docs/01-opportunity-brief.md](docs/01-opportunity-brief.md) | The gap, personas, opportunity, hypothesis, flagged assumptions |
| [docs/02-prd.md](docs/02-prd.md) | Main deliverable: Problem Formulation, Solution Definition, Technical Implications |
| [docs/03-event-model.md](docs/03-event-model.md) | The intent to production mechanism, walked command by command |
| [diagrams/](diagrams/) | Mermaid sources for the path to production (before/after), the event model, and communication flow, plus the success-metrics chart (light/dark SVG) |
| [slides/deck.md](slides/deck.md) | Marp deck version of the PRD, timed for a live readout |
| [mockups/index.html](mockups/index.html) | Low-fidelity before/after wireframe of the developer flow |
| [appendix/ai-prompts.md](appendix/ai-prompts.md) | Representative AI prompts used to accelerate research and pressure-test trade-offs |

## Personas

- **Maya**: Product Engineer, Tier-2 service owner. Wants to ship without becoming an infra expert.
- **Dev**: Senior Platform Engineer, the skeptic. Wants to see exactly what automation will do before it does it.
- **Sam**: Platforms Lead. Wants velocity that doesn't spend the reliability budget.

## How this was built

Frameworks applied directly (opportunity framing, jobs-to-be-done personas, event modeling, Marp decks, low-fidelity wireframes). Benchmark figures (DORA change-failure rates, Copilot acceptance rates, platform engineering adoption stats) are sourced and cited inline in the PRD. Anything without a citation is explicitly flagged as an assumption or illustrative placeholder, not asserted as fact.

## Quick start

- Read the PRD first, it's the main deliverable.
- View the deck with any Marp renderer (`npx @marp-team/marp-cli slides/deck.md -o slides/deck.html`) or read it as markdown.
- Open `mockups/index.html` directly in a browser.

## License

All rights reserved.
