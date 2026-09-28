# Appendix: AI prompts used

Per the exercise instructions, a few representative prompts used to accelerate research and structuring. The strategic logic, prioritization calls, and trade-offs in the PRD are mine. AI was used to pressure-test them and pull real external benchmarks, not to generate the recommendation.

1. **Benchmark research:** *"Find current, sourced data on platform engineering toil metrics, DORA benchmarks for change-failure rate and deployment frequency by performance tier, and adoption stats for golden-path platforms like Backstage/Crossplane. I need real numbers with sources, not estimates."*
   Used to ground the North Star metrics and the "consistency is the reliability argument" claim in real DORA data rather than an assumed number.

2. **Trade-off pressure test:** *"I'm planning to automate Provisioning and Traffic Management first and explicitly delay Security & Compliance automation for a quarter, even though it's also high-toil. Push back on this: what's the strongest argument that I have the priority backwards?"*
   Used to stress-test the build-vs-delay call before committing to it in the PRD; the counterargument (security toil is also real pain) is acknowledged in the writeup rather than hidden.

3. **Mechanism check via event modeling:** *"Walk the Service Intent flow as an event model, command, event, read model, actor, from intent submission to production deploy. Where are the implicit reads or missing human checkpoints that the prose version doesn't surface?"*
   This is what surfaced that the intent form itself is a read-model dependency (Maya needs to see valid tiers/runtimes before submitting), and that rejection needed to be modeled as a first-class event rather than an implicit retry.

4. **Compression pass:** *"This PRD section is running long for a 120-minute exercise. Cut it by roughly a third without losing the actual argument. Keep the number, drop the hedging."*
   Used repeatedly to keep the memo close to the intended time-box rather than let the AI-assisted research turn into an overlong document.
