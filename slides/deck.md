---
marp: true
theme: default
paginate: true
size: 16:9
---

# Intent-Based Developer Platform
### A 6-month plan to close the "wall of toil" between code and production

Nicholas Pavez · DevEx / Platforms

---

## The problem, in one line

Engineers write code fast. Then they hand-glue four mature-but-disconnected capabilities (**Provisioning, Security & Compliance, Observability, Traffic Management**) with thousands of lines of boilerplate before anything reaches production.

The same problem, solved four different ways, every single launch.

---

## Where the friction actually is

As the PM here: data gathering and analysis, not asserting from a label. All four capabilities are necessary in the SDLC; the question is which specific step, inside any of them, costs the most time. This is a case study, so the numbers below are stated assumptions, a hypothesis to test, not a finding:

Illustratively: the provisioning approval queue and the LB/DNS cutover score highest (every launch, multi-day, fully blocking). Security template selection scores low (minutes, non-blocking); net-new security requests score high but narrow (~30% of launches). Confirmed with real data, not assumed:
- CI/CD event logs → time from "scaffolded" to "first prod deploy"
- Auto-classified platform tickets → where the manual glue work lands, at the exact step
- Git history → how much of the boilerplate is copy-paste, not novel

---

## What success looks like

A developer with a tested service goes from code-complete to live, secured, and observable, **without opening a ticket or reading a Terraform doc**, for the common case.

Not: the AI writes any config.
It's: the boilerplate disappears for the 80% case; the 20% case still routes to a human.

---

## Three North Star metrics, with targets

1. **Self-serve completion rate**: 40% at 90 days, 70% at 12 months
2. **Time-to-first-deploy**: under 1 day by 12 months, down from an illustrative ~4.5-day baseline
3. **Change-failure rate on generated config**: at or below the manual baseline, every checkpoint

*(plus a secondary trust signal: agent proposal acceptance rate, starting below the ~30-40% Copilot benchmark)*

![Projected impact: self-serve completion and time-to-first-deploy over 12 months. Placeholder numbers.](../diagrams/success-metrics-chart.svg)

*All numbers placeholders until Week 1 instrumentation and pilot data replace them.*

---

## The interface: an AI intent layer, not a rigid form

Developers arrive with one of three things. The platform reads whichever one they have:

- **A blurb**: "Checkout service, Python, ~1k RPS."
- **A POC repo**: statically read, never executed, to infer the same fields.
- **A legacy repo**: diffed against current templates for a modernization path.

All three converge on one structured intent record, shown back for confirmation before anything is generated.

---

## Constraints come from the business, not the developer

Notice what's missing above: tier, region, budget, owner. A developer's description should never set these.

They're **looked up automatically** from whatever system already owns them, a service catalog, a cost tool, an org chart, a network policy engine, the moment the developer's team is known:

- **Risk tier**: set by Security/Compliance, per team, not per service
- **Budget**: a cost ceiling from FinOps tooling, caps what the agent can size
- **Owner**: the accountable sub-department, drives review routing
- **Routability**: which regions the team is permitted to touch at all

Shown in the confirm screen as fixed, not editable. A conflict (traffic that would blow the budget, a region the team isn't routable into) is surfaced by name, never silently resolved.

---

## The safety argument isn't the input format

1. Extraction produces a **draft**, never a decision.
2. The developer **confirms or corrects** the understanding, in plain language, before anything downstream happens.
3. Only the **confirmed** record drives composition from pre-approved modules. Raw text or code never does.
4. A human still reviews and merges every generated PR.

The guardrail is the confirm-then-compose sequence, not what a developer is allowed to say.

---

## Build first, delay deliberately

**Build first (input):** the blurb path. Fastest to ship, covers the common case.
**Delay:** POC-repo and legacy-repo ingestion. Bigger lift, more trust required first.

**Build first (friction point):** whatever Week 1 data ranks highest, Tier-2 only. Not a capability, illustratively the provisioning queue and LB/DNS cutover.

**Guardrails ship on day one**, not later: template selection is part of composition itself. What's delayed is only generating *novel* security policy from scratch, a new decision, not toil.

**Delay:** full observability automation, other runtimes. Not because it matters less: lowest-severity friction, tedious but non-blocking.

---

## Speed and stability aren't in tension

Every hand-authored Terraform file is a fresh source of drift.
Every agent-composed deploy comes from the same audited template.

**Consistency is the reliability argument**, not a trade against it.

---

## The developer's Tuesday afternoon

**Today:** finish the service → open 4 tools → spend the rest of the week hand-writing Terraform, IAM, LB config before staging.

**After:** describe it in one sentence → agent shows what it understood → confirm → generated PR in minutes, plain-language rationale attached → one team review → merge. **Production by Tuesday evening.**

(Wireframe: [mockups/index.html](../mockups/index.html))

---

## 4-week MVP: one team, one tier

| Week | What happens |
|---|---|
| 1 | Instrument signals; confirm friction ranking *and* that the team's metadata (tier/budget/owner/routability) is clean |
| 2 | Build blurb-to-intent extraction + composition, propose-only |
| 3 | Pilot team runs 2-3 real launches through it; human merges every PR |
| 4 | Measure time-to-first-deploy vs. their last 3 launches; decide: expand or hold |

Real test: **does the team use it again without being told to?**

---

## Earning trust with the skeptics

- **Shows its understanding first**: restates what it inferred before generating anything
- **Propose, never auto-merge**: through the pilot and the quarter after
- **Every PR carries its rationale**: nothing is hidden behind the interface
- **Business constraints shown, never silent**: tier, budget, owner, routability labeled as fixed inputs, not agent decisions
- **Escape hatch always visible**: raw Terraform is inspectable, editable
- **Autonomy earned in stages**: auto-merge, if ever, starts on the single lowest-risk field, after a clean quarter

---

## Adoption: launch it like a product, not a mandate

- One credible senior engineer as internal champion
- Publish the pilot's real numbers: one team's data, labeled as such
- A named, discoverable "golden path" with runnable examples
- Opt-in for two quarters before any team is required to use it

---

## What I'd want to be wrong about

The whole plan assumes the four underlying capabilities each already expose a machine-readable interface (API, Terraform module, or CRD). If they don't, phase 1 is "build the interface," not "build the agent," and the timeline moves right by a quarter. I'd confirm this in week 1, before writing a line of the composition logic.
