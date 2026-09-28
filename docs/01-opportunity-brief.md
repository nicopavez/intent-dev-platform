# Opportunity Brief: Intent-Based Developer Platform

*DevEx / Platforms team · grounded in Yahoo's public Platforms org (Cloud Runtime, Security, Messaging) as the illustrative setting*

## The gap

Yahoo's engineers are fast at writing code and slow at shipping it. Four platform capabilities (Provisioning, Security & Compliance, Observability, and Traffic Management) are each mature on their own, but nothing connects them. A developer who wants to ship a simple microservice has to become a part-time expert in all four: write Terraform for clusters and namespaces, hand-roll IAM policy and secrets, wire up dashboards and alert thresholds, and configure load balancers, DNS, and routing rules. That's thousands of lines of boilerplate before a single business line of code reaches production.

That toil is a tax on every team, every time they ship something new. It's also inconsistent: four engineers solving the same "stand up a Tier-2 service" problem produce four different configurations, each a slightly different source of drift and risk.

## Why now

Three things make this solvable in the next two quarters that weren't true before:
- **The underlying capabilities are already mature.** This isn't a rebuild, it's a translation problem. Provisioning, Security, Observability, and Traffic Management each already expose APIs or IaC modules; nothing here requires re-architecting the infrastructure itself.
- **The business-side governance already exists somewhere too.** Risk tier, budget, ownership, and routability for a given team aren't new decisions this platform has to invent; they live in a service catalog, a cost tool, an org chart, or a network policy engine already. The same translation logic applies one layer up: read them, don't originate them.
- **LLMs are now reliable enough to understand messy developer input (a blurb, a POC, a legacy repo) and turn it into a structured, confirmable intent record**, provided the AI layer is scoped to compose pre-approved, human-authored building blocks, within business-fixed constraints, rather than generate infrastructure config from scratch or let a developer's self-report set their own risk tier. That distinction is the whole safety argument (see PRD, Part 3, Functional Requirements).

## Personas

- **Maya, Product Engineer (Tier-2 service owner).** Ships a new Python microservice every 4-6 weeks. Knows her application code cold, has copy-pasted Terraform from the last service that looked similar, and has caused one near-incident from a misconfigured security group she didn't fully understand. Her job to be done: *"When I've finished a service, let me get it safely into production without becoming an infra expert."*
- **Dev, Senior Platform Engineer (the skeptic).** Has hand-built internal tooling to work around the platform's gaps for eight years. Trusts what he can read and roll back, not what a model asserts. His job to be done: *"When something touches production, let me see exactly what it will do before it does it, and let me override it."*
- **Sam, Platforms Lead (my counterpart, accountable for reliability).** Owns the SLA for the four underlying capabilities and answers for any incident traced to automation. Job to be done: *"When we accelerate developer velocity, don't do it by trading away my error budget."*

## Opportunity

Close the gap between "code is done" and "code is live," not by replacing the four capabilities, but by giving developers one intent-based interface that composes them correctly by default, with senior engineers still in control of what "correct" means.

## Hypothesis

If we let a developer describe *what* they're deploying, in whatever form they already have it (a one-line blurb, a POC repo, a legacy service), instead of requiring them to specify *how* to wire it (LB rules, IAM, dashboards) or what risk tier, budget, and region their own team operates under, and have an AI-Agentic Layer turn that description plus the business's own fixed constraints into pre-approved, policy-gated configuration that's reviewable as a diff before it ships, then time-to-first-deploy for new services will drop materially, self-serve completion will rise, and change-failure rate attributable to config will not regress, because the agent is only ever recombining infrastructure that senior engineers have already vetted, inside limits the business already set.

## Assumptions (flagged, not verified)

- The four capabilities each expose some machine-readable interface today (API, Terraform module, or CRD) that an orchestration layer can call, not just runbooks. If not, phase 1 becomes "build the API" rather than "build the agent," and the timeline in the PRD moves right by a quarter.
- Team-level risk tier, budget, ownership, and routability already exist in some system of record. If they're missing, stale, or inconsistent, the same delay applies: phase 0 becomes reconciling that data before any of it can be trusted.
- "Tier-2" is an existing internal service classification with meaningfully different requirements than Tier-1/Tier-0 services, used here as the starting wedge because it's common and low-blast-radius.
- One squad of 4-5 engineers for one quarter is enough to ship a working MVP for one service tier, not a general-purpose platform. Scope in the PRD is sized to that constraint, which is also why POC and legacy-repo ingestion are deferred past the MVP (see PRD, Part 2, Out of scope and non-goals) and the blurb path ships first.
