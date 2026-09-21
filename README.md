# Maxim Kushnir

Backend and systems engineer. I build deterministic, event-sourced backends in async Python, and reach for Rust when a benchmark says it's worth it.

Since December 2022 I've been the sole engineer of [Ventute](https://ventute.com), an AI business-simulation platform in production with real users. I designed and wrote the whole backend, from the simulation kernel to deployment and monitoring. I'm based in Varese, Italy, and moving to Birmingham, UK in November 2026.

## About me

What I care about most is systems you can trust: if something happened, you should be able to replay it and get exactly the same result, and if a rule matters, the code should make breaking it impossible rather than hoping a test catches it. Most of my work on Ventute comes back to that idea.

I also like understanding the business side of what I build. I studied Business alongside Maths and Computer Science at A-level, and Ventute is at heart a financial and business model that has to stay consistent turn after turn. That's a big part of why I'm interested in fintech.

I speak English, Italian and Ukrainian.

## What I've built at Ventute

**A deterministic simulation kernel.** One event-sourced kernel is the only thing allowed to change state, time and money. Everything unpredictable, like random numbers, model output and clock reads, is resolved once and written to an append-only log. A pure reducer then folds that log back into state, so any past turn can be replayed bit-for-bit. Golden tests carry invariants that must still hold after a snapshot is regenerated, so a bug can't quietly become the new baseline.

**Declarative engine orchestration.** The simulation used to run through a 1,000-line, hand-ordered sequence of engines. I replaced it with 21 engines that each declare their phase, the state they read, the state they own and what they depend on. The execution order is derived with Kahn's algorithm, and cycles or contradictions are rejected before anything runs. Engines only propose changes; a single writer applies them and checks ownership. This caught real bugs, including an alphabetical tie-break that let one engine silently overwrite revenue from closed enterprises.

**A native Rust extension (PyO3 + Rayon).** Monte-Carlo forecasting, optimisation and calibration run in Rust with the GIL released. Each rollout derives its random stream from its run index, so results are identical however the threads get scheduled. I also benchmarked porting the event fold to Rust, and dropped it: restructuring the Python version was the better trade.

**Release safety and operations.** Prometheus and Grafana across the service and simulation paths, secrets in AWS Secrets Manager, feature flags for engine rollouts, and tenant isolation at the data-access layer. My favourite fix: a canary gate that had been shipped and reviewed but never actually ran, because it treated missing data as a pass. I added a traffic metric, fixed a Prometheus label bug, handled the 0/0 case, and proved the gate works by deploying a deliberately broken image.

## How I work

- I'd rather make a mistake impossible than detect it later: declared ownership, derived ordering, pure functions.
- I measure before I optimise, and I'm happy to throw away work when the numbers say so.
- When I fix a bug, I look for the whole class of bug. Ventute has a CI-checked taxonomy of bug classes for exactly this.

## Open source

- **vLLM** [#51560](https://github.com/vllm-project/vllm/pull/51560) (open): model initialisation now fails fast with a clear error on unsupported GPU architectures, instead of an opaque runtime failure later on.

## Standalone examples

Small, runnable versions of patterns from Ventute, with the domain logic removed:

- [event-driven-replay-kernel](https://github.com/m4xkushnir/event-driven-replay-kernel): async event-sourcing kernel with hash-verified replay
- [deterministic-ai-guardrail](https://github.com/m4xkushnir/deterministic-ai-guardrail): parse, validate and clamp layer between an LLM and application state
- [pyo3-rust-py-bridge](https://github.com/m4xkushnir/pyo3-rust-py-bridge): minimal PyO3 + Rayon extension for parallel CPU-bound work

## Stack

**Languages:** Python 3.11 (async), Rust, C++, SQL, Java
**Backend:** FastAPI, Pydantic v2, async SQLAlchemy, asyncpg, Alembic, GraphQL
**Data:** PostgreSQL (Neon), Redis
**Infrastructure:** Docker, Kubernetes, Terraform, Prometheus, Grafana, feature flags

## Background

- Cambridge International A-Levels in Mathematics, Computer Science and Business
- ITIS Informatico, technical informatics track (algorithms, data structures, low-level architecture, OOP)
- University of Helsinki: Full Stack Open (grade 5/5), DevOps with Kubernetes, Java Programming I & II
- Harvard CS50's Introduction to AI with Python
- Saylor Academy CS107: C++ Programming

maxim955827848@icloud.com · [LinkedIn](https://www.linkedin.com/in/maxim-kushnir-7a78993a2/)
