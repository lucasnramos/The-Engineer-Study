# Full-Stack Engineer Study & Portfolio Plan

*24-week roadmap focused on backend depth, production engineering, and interview readiness*

> **Designed for your current profile**
> Senior-capable frontend engineer (strongest in Angular/TypeScript, solid React) building deeper skills in Node.js, databases, security, observability, system design, performance, and full-stack production ownership.

**Primary outcome: code and engineering artifacts you can show in interviews—not just completed lessons.**

Prepared from your self-assessment and resume • November 2026

# 1. Executive summary

Your biggest opportunity is not another frontend framework. Your resume already demonstrates years of production Angular/React work, frontend architecture, code review, mentoring, responsive UI work, and real client delivery. The self-assessment shows the biggest gaps are underneath the application layer: relational data, backend runtime behavior, security, failure handling, observability, system design, and performance.

> **Plan principle**
> Spend most of the next six months turning those weak areas into visible evidence. Every major topic below ends in code, a benchmark, a design artifact, or a test that can live in a public portfolio repository.

| **Priority** | **Capability**                                               | **Why it matters now**                                                                                            |
|--------------|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| P0           | SQL + relational data modeling                               | Largest foundational gap; affects APIs, ORMs, performance, consistency, and system design.                        |
| P0           | Backend security                                             | Required for credible senior full-stack ownership; currently low confidence across the security surface.          |
| P0           | Node.js runtime fundamentals                                 | Your Node experience is practical but framework-led; deepen event loop, streams, processes, memory, and shutdown. |
| P1           | Reliability: transactions, idempotency, retries, concurrency | Turns CRUD knowledge into production backend engineering.                                                         |
| P1           | Observability + production operations                        | Needed to diagnose real systems rather than only debug locally.                                                   |
| P1           | System design                                                | Connects data, reliability, scaling, and trade-offs; important for senior interviews.                             |
| P2           | Modern React + accessibility + performance                   | Targeted refresh where your practical experience is stronger than your current mental models.                     |

Recommended pace: 6–8 focused hours per week. If work or life reduces that, preserve the order rather than the calendar. The plan is intentionally 24 weeks, but a 30–32 week version is completely valid.

# 2. What you should have at the end

- One polished full-stack capstone repository with a React + TypeScript frontend, NestJS backend, PostgreSQL, containers, CI, tests, observability, security controls, and documented design decisions.
- A /labs directory or companion repository showing Node runtime experiments: streams/backpressure, event-loop blocking, worker threads, graceful shutdown, and memory profiling.
- A SQL performance lab with schemas, queries, EXPLAIN plans, index before/after comparisons, transaction examples, and an N+1 reproduction/fix.
- A security folder containing a lightweight threat model, authorization tests, security headers, dependency scanning, and notes from selected OWASP Juice Shop exercises.
- A load-testing suite (k6) with a baseline, bottleneck found, change made, and measured result.
- Architecture documentation: context/container diagram, data model, failure modes, ADRs, and one short postmortem from an intentionally injected failure.
- An interview notebook: DSA patterns, SQL drills, system design prompts, frontend/backend questions, and 6–8 STAR(R) stories based on your actual career.

> **Portfolio rule**
> Do not optimize for “many repositories.” Optimize for a small number of repositories that make your engineering thinking inspectable. A hiring manager should be able to see what you designed, measured, broke, fixed, and learned.

# 3. Main capstone: Household Inventory & Replenishment Platform

Use the household-items project you already started rather than abandoning it for a new toy app. Evolve it into a production-style system whose complexity is intentional and documented.

## 3.1 Suggested scope

| **Area**     | **Features / evidence**                                                                                                                      |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| Identity     | Users, households, membership, roles (owner/member/read-only), secure login/session flow.                                                    |
| Inventory    | Items, quantities, units, categories, locations, minimum stock, expiration dates.                                                            |
| Stock ledger | Every adjustment recorded as an append-only stock movement; current quantity derived or transactionally maintained.                          |
| Shopping     | Generated shopping list when stock crosses threshold; manual additions and completion.                                                       |
| API          | Pagination, filtering, sorting, validation, consistent errors, versioning policy, idempotent mutation where appropriate.                     |
| Async work   | Low-stock notification or import job processed by a background worker with retry policy.                                                     |
| Performance  | Indexes based on measured queries; load tests; cache only after a measured need.                                                             |
| Operations   | Structured logs, request/correlation IDs, metrics, traces, health checks, graceful shutdown.                                                 |
| Security     | Authorization at resource boundary, secure session/token approach, rate limiting, headers, input validation, dependency scanning.            |
| Frontend     | React + TypeScript; TanStack Query for server state; URL-backed filters; optimistic update; accessible forms/dialogs; performance profiling. |

Suggested stack (keep it boring): NestJS + TypeScript, PostgreSQL, TypeORM initially, React + TypeScript + Vite, TanStack Query, Docker Compose, GitHub Actions, OpenTelemetry, and k6. Redis and a job queue should be introduced only when the phase calls for them.

> **Important constraint**
> Use TypeORM for normal application code, but deliberately write and inspect raw SQL during the database phase. The goal is to prevent the ORM from hiding the skill you need to build.

## 3.2 Repository structure

```text
apps/web              React client
apps/api              NestJS HTTP API
apps/worker           Background job worker (added later)
packages/contracts    Shared API types/schemas where appropriate
infra/                Docker Compose + local observability config
labs/node-runtime/    Event loop, streams, workers, memory exercises
labs/sql/             SQL, EXPLAIN, indexes, transactions, N+1
load/                 k6 scripts and results
docs/architecture/    diagrams, ADRs, threat model, failure modes
docs/interview-notes/ optional public-safe study notes
```

# 4. Weekly operating model

| **Session**      | **Time**      | **What to do**                                                                            |
|------------------|---------------|-------------------------------------------------------------------------------------------|
| Learn            | 2 × 45–60 min | Read/watch only what supports the current milestone. Take short notes in your own words.  |
| Build            | 2 × 75–90 min | Implement the phase deliverable. Commit small, explainable increments.                    |
| Measure / verify | 45–60 min     | Run tests, EXPLAIN, profiler, security check, load test, or trace—depending on the phase. |
| Interview prep   | 2 × 30–45 min | DSA/SQL practice early; add system design and mock interviews later.                      |
| Weekly review    | 20 min        | Write: what I learned, what surprised me, what I can now explain without notes.           |

> **Avoid passive study**
> A resource is “done” when it changes code or your explanation of code. You do not need to finish every linked guide end-to-end.

# 5. Phase 1 — SQL and relational data (Weeks 1–5)

Goal: move from “I can use an ORM” to “I can design a relational model, predict common query costs, and diagnose data-access problems.” This is the highest-priority phase.

## Week 1 — SQL fundamentals and schema design

- Work through PostgreSQL tutorial sections on tables, queries, joins, aggregates, foreign keys, and transactions.
- Model the capstone entities on paper first: users, households, memberships, items, locations, stock movements, shopping-list entries.
- Create migrations. Use NOT NULL, UNIQUE, CHECK, foreign keys, and appropriate delete behavior deliberately.
- Write 10–15 raw SQL queries without TypeORM: joins, group-by, aggregate, subquery/CTE, update, transaction.

Code evidence: migrations + `docs/data-model.md` + `labs/sql/basics.sql`.

## Week 2 — Joins, aggregates, N+1, and ORM visibility

- Complete targeted PostgreSQL Exercises on joins and aggregates.
- Reproduce an N+1 query in TypeORM, capture the SQL/logs, then fix it with an appropriate join/eager query/batched approach.
- Add an integration test that asserts the result, and a short note explaining why the first implementation scales poorly.

Code evidence: `labs/sql/n-plus-one.md` with query counts before/after.

## Week 3 — Indexes and query plans

- Create a realistic dataset (for example 100k–500k stock movements).
- Run key queries with `EXPLAIN (ANALYZE, BUFFERS)` before indexes.
- Add one or two justified indexes; rerun and save the plans/results.
- Demonstrate at least one useless or harmful index and explain why it should not exist.

Code evidence: benchmark script + saved query plans + before/after table in README.

## Week 4 — Transactions, isolation, and concurrency

- Implement an inventory adjustment transaction that writes the stock movement and updates/validates current stock atomically.
- Create two concurrent requests that try to modify the same item. Observe behavior.
- Study PostgreSQL isolation basics and decide whether optimistic locking, row locking, or another approach is appropriate for this feature.
- Add a concurrency-focused integration test.

Code evidence: transaction service + test showing a race and the chosen fix.

## Week 5 — Data design review

- Review normalization vs intentional denormalization. Keep a derived value only if you can explain consistency strategy.
- Document indexes and expected access patterns.
- Do a 30-minute “database interview” aloud: schema design, N+1, index trade-offs, transactions, isolation, EXPLAIN.

> **Exit criterion**
> You can explain why each major table and index exists, diagnose an N+1 query, read a basic EXPLAIN plan, and implement a transaction that protects an invariant.

## Resources — Phase 1

- [PostgreSQL official tutorial](https://www.postgresql.org/docs/18/tutorial.html) — Relational concepts, joins, aggregates, foreign keys, transactions, and window functions.
- [PostgreSQL EXPLAIN documentation](https://www.postgresql.org/docs/18/sql-explain.html) — Learn what the planner chose and compare scans/joins/costs with actual execution.
- [PostgreSQL Exercises](https://www.pgexercises.com/) — Hands-on SQL practice from basic queries through joins, aggregates, windows, and recursive queries.

# 6. Phase 2 — Node.js runtime, not just NestJS (Weeks 6–8)

Goal: understand the runtime behavior underneath the framework, especially the areas your assessment marked low: streams, workers, child processes, memory, event-loop behavior, and shutdown.

## Week 6 — Event loop, async behavior, and CPU blocking

- Write a tiny HTTP server with one I/O-heavy route and one CPU-heavy route.
- Measure what happens to unrelated requests while the CPU-heavy route runs.
- Move the CPU-heavy work to `worker_threads`; compare latency.
- Be able to explain microtasks, timers, I/O callbacks, and why “async function” does not make CPU work non-blocking.

## Week 7 — Streams, backpressure, and large files

- Generate a multi-GB or sufficiently large CSV/file locally.
- Implement version A with whole-file loading and version B with streams.
- Track memory usage and throughput. Add a transform stream and demonstrate backpressure behavior.
- Write a short README: “Why streams matter in Node.”

## Week 8 — Processes, shutdown, and memory diagnostics

- Spawn a child process for a safe local command and handle exit/error output.
- Add SIGTERM/SIGINT handling to the API: stop accepting work, finish in-flight work, close DB/queue connections, then exit.
- Create a deliberate memory leak (for example retained listeners/objects), capture heap usage, fix it, and record the evidence.

> **Exit criterion**
> You can explain when Node is concurrent, when it is not, how streams reduce memory pressure, when workers help, and what a graceful container shutdown actually does.

## Resources — Phase 2

- [Node.js Learn](https://nodejs.org/learn) — Official learning hub covering async work, concurrency, file system, package management, diagnostics, testing, and security.

# 7. Phase 3 — API design and reliability (Weeks 9–11)

Goal: graduate from “REST CRUD” to APIs that behave predictably under retries, validation failures, concurrency, and partial failure.

## Week 9 — HTTP semantics and API contracts

- Standardize error responses and document 400 vs 401 vs 403 vs 404 vs 409 vs 422 vs 500 usage.
- Implement cursor or page-based pagination with stable sorting. Document why the chosen approach fits your dataset.
- Add request validation at the boundary. Treat TypeScript types as compile-time only; validate all external data at runtime.
- Generate/maintain OpenAPI documentation and include examples.

## Week 10 — Idempotency and duplicate side effects

- Choose one mutation such as “create shopping order” or “record stock import.”
- Accept an idempotency key, persist the request/result relationship, and return the original result on retry.
- Write an integration test that sends the same request multiple times and proves only one side effect occurs.
- Document key lifetime, conflict behavior, and what happens if the first request crashes partway through.

## Week 11 — Async work, retries, and the transactional boundary

- Add a background job for low-stock notification, CSV import, or shopping-list generation.
- Define bounded retries with backoff and a terminal failure state/dead-letter path.
- Implement either an outbox pattern or a clearly documented simpler consistency approach so a DB commit and async event cannot silently diverge.
- Make retry-safe handlers idempotent.

> **Exit criterion**
> You can discuss retries without accidentally creating duplicate work, explain where a transaction begins/ends, and design an API failure response that clients can act on.

# 8. Phase 4 — Web/backend security (Weeks 12–14)

Goal: turn the broad 1/4 security confidence into a practical baseline you can defend in an interview and demonstrate in code.

## Week 12 — Authentication, sessions, and authorization

- Choose one authentication model and document why. For a browser app, strongly consider secure HttpOnly cookie-based sessions or a well-understood BFF/token design rather than casually storing long-lived tokens in localStorage.
- Implement household/resource authorization. A user who belongs to Household A must not read or mutate Household B by guessing IDs.
- Write negative authorization tests first: anonymous, wrong role, wrong household, stale/invalid session.

## Week 13 — Common web vulnerabilities

- Study XSS, CSRF, SQL injection, SSRF, CORS, CSP, secure cookies, password hashing, and secrets management.
- Add security headers, strict CORS policy, rate limiting on sensitive endpoints, and dependency scanning in CI.
- Verify parameterized queries / ORM safety. Add explicit tests for malicious-looking input where useful.

## Week 14 — Threat model + controlled security lab

- Create a one-page threat model: assets, trust boundaries, attackers, likely abuse cases, mitigations.
- Run OWASP Juice Shop locally and complete a small, representative set of challenges (for example broken access control, XSS, injection, authentication/session). Keep notes focused on the underlying bug and fix—not just the exploit steps.
- Apply at least three lessons back to the capstone and commit the hardening changes.

> **Exit criterion**
> You can explain authentication vs authorization, session/token storage trade-offs, XSS/CSRF/CORS at a practical level, and prove authorization with tests.

## Resources — Phase 4

- [OWASP Web Security Testing Guide](https://wstg.owasp.org/) — A practical testing methodology for identity, auth, authorization, sessions, input validation, client-side security, and more.
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) — Focused implementation guidance for sessions, authentication, CSRF, XSS, secrets, and related controls.
- [OWASP Juice Shop](https://owasp.org/projects/juice-shop) — Intentionally vulnerable Node/Angular application for hands-on web security practice.
- [MDN CORS security guide](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CORS) — Clear browser security model and safe CORS configuration guidance.

# 9. Phase 5 — Containers, observability, and performance (Weeks 15–18)

Goal: make the system operable. You should be able to answer “How would you know this is broken?” without saying “attach a debugger.”

## Week 15 — Docker and local production-like environment

- Containerize API and web app; run API, PostgreSQL, and any worker through Docker Compose.
- Use multi-stage builds where appropriate, non-root runtime user, explicit health checks, and environment-driven configuration.
- Verify SIGTERM behavior from Phase 2 inside the container.

## Week 16 — Structured logging and correlation

- Emit structured JSON logs with timestamp, level, request ID/correlation ID, route, status, duration, and error context.
- Propagate correlation ID from inbound request to worker job and outbound calls where possible.
- Create one production-like incident and solve it primarily from logs.

## Week 17 — Metrics and distributed tracing

- Instrument the API with OpenTelemetry.
- Capture request latency, error rate, throughput, DB duration, and background-job metrics.
- Trace a request from HTTP entry through service/database and into async work where feasible.
- Define 3–5 useful alerts/SLO-style thresholds, even if they run only locally.

## Week 18 — Load testing and measured optimization

- Create k6 scenarios for read-heavy and write-heavy paths.
- Establish a baseline. Do not optimize before measuring.
- Find one real bottleneck: query/index, N+1, serialization, event-loop block, frontend waterfall, or other measured issue.
- Fix it and publish before/after numbers with methodology and caveats.

> **Exit criterion**
> A reviewer can run the stack locally, see health/logs/traces, execute a load test, and inspect a documented performance improvement.

## Resources — Phase 5

- [Docker Node.js guide](https://docs.docker.com/guides/nodejs/) — Official guide for containerizing a TypeScript/Node application and running tests in containers.
- [OpenTelemetry JavaScript getting started](https://opentelemetry.io/docs/languages/js/getting-started/) — Add traces/metrics to Node.js and learn the telemetry model.
- [Grafana k6 getting started](https://grafana.com/docs/k6/latest/get-started/) — Write repeatable JavaScript/TypeScript load tests and define performance thresholds.

# 10. Phase 6 — Modern React, accessibility, and browser performance (Weeks 19–21)

Goal: modernize the parts of React/frontend knowledge where your production experience is already strong but your assessment exposed conceptual gaps.

## Week 19 — Effects and server state

- Audit the capstone for `useEffect`. Classify each effect: external synchronization, derived state that should move to render, user event that should move to a handler, or data fetching that belongs in a server-state abstraction.
- Use TanStack Query for remote/server state: caching, invalidation, loading/error states, mutations, and one optimistic update with rollback.
- Keep UI/client state separate from server state and explain the difference in the README.

## Week 20 — Accessibility

- Run keyboard-only flows for login, item creation, inventory adjustment, filter/search, and modal/dialog interactions.
- Fix semantic structure, labels, focus order, focus return, error announcements, and contrast issues.
- Add automated accessibility checks if convenient, but include at least one manual checklist because automation cannot prove full accessibility.

## Week 21 — Browser and frontend performance

- Measure bundle size and identify the largest dependencies/chunks.
- Use route/component lazy loading where justified; verify the generated chunks rather than only reorganizing folders.
- Profile one slow interaction and one initial-load path using browser tooling.
- Record relevant Core Web Vitals or equivalent local measurements and document the change.

> **Exit criterion**
> You can explain effects as synchronization, server state as remote cached state, demonstrate an optimistic update, pass a keyboard flow, and show one measured frontend performance improvement.

## Resources — Phase 6

- [React: Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects) — Official mental model: effects synchronize with systems outside React and need cleanup when appropriate.
- [React: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) — Excellent corrective guide for overusing effects for derived state and event-driven logic.
- [TanStack Query optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates) — Hands-on server-state mutation, cache update, rollback, and invalidation patterns.
- [web.dev Accessibility](https://web.dev/accessibility) — Structured accessibility course and reference for semantic HTML, forms, focus, and testing.

# 11. Optional Angular refresh lab (2–4 evenings, parallel)

Angular is already your strongest framework, so do not allocate a full phase. Use one small feature or branch to update modern concepts that were shaky in the assessment.

- Build a small standalone feature with standalone components/directives/pipes and no feature NgModule.
- Model local state with `signal`, derived state with `computed`, and a justified `effect` only for non-reactive side effects.
- Use Angular DevTools or Chrome profiling to observe change detection and one lazy/deferred-loaded feature.
- Write one page comparing signals, RxJS streams, and React state—not as “equivalents,” but by use case and semantics.

## Resources — Angular refresh

- [Angular Signals guide](https://angular.dev/guide/signals) — Official dependency-tracking model for `signal`, `computed`, and effects.
- [Angular standalone migration/reference](https://angular.dev/reference/migrations/standalone) — Explains standalone components/directives/pipes and reducing NgModule dependence.
- [Angular performance best practices](https://angular.dev/best-practices/performance) — Current Angular guidance for loading performance, change detection, profiling, and defer/lazy strategies.

# 12. Phase 7 — System design and capstone hardening (Weeks 22–24)

Goal: convert the implementation experience from the previous phases into reusable architectural reasoning and interview language.

## Week 22 — Architecture documentation and ADRs

- Create a context diagram and container diagram.
- Write 3–5 ADRs for decisions you actually made: authentication approach, stock ledger model, queue/retry design, idempotency, caching choice, or database indexing strategy.
- Each ADR should contain context, options considered, decision, consequences, and what would make you revisit it.

## Week 23 — Failure modes and scale thought experiments

- Document behavior when PostgreSQL is slow/unavailable, worker crashes, Redis/cache is down (if used), external provider times out, or a request is retried.
- Estimate how the system changes at 10× and 100× current traffic. Avoid adding infrastructure unless a bottleneck justifies it.
- Practice explaining consistency, caching, queueing, replication, partitioning, and horizontal scaling using your capstone as the example.

## Week 24 — Portfolio release and mock interview

- Polish README: problem, architecture, how to run, key engineering decisions, testing strategy, screenshots, observability example, load-test result, and known trade-offs.
- Tag a v1.0 portfolio release.
- Record yourself doing a 45-minute system-design walkthrough of the project without reading the README.
- Run one full mock loop: coding + system design + behavioral.

> **Exit criterion**
> The repository tells a coherent engineering story: requirements → design → implementation → tests → failures → measurement → trade-offs. You can discuss that story without pretending the system operates at imaginary scale.

## Resources — Phase 7

- [System Design Primer](https://github.com/donnemartin/system-design-primer) — Broad, free coverage of scalability, consistency, load balancing, caching, queues, databases, and interview exercises.
- [Tech Interview Handbook: System Design](https://www.techinterviewhandbook.org/system-design/) — Interview-oriented framework and curated system-design resources for experienced candidates.

# 13. Parallel interview-preparation track

Because algorithms are a self-identified interview weakness, interview prep should run in parallel from Week 1. Do not wait until you are applying. The goal is fluency under time pressure, not competitive-programming depth.

> **Language choice**
> Use JavaScript or TypeScript for coding interviews unless a target role specifically expects another language. You already think fluently in JS/TS, and interview preparation should test problem solving—not your memory of a newer language’s standard library.

## 13.1 DSA progression

| **Weeks** | **Focus**                                                                            | **Target**                                                                      |
|-----------|--------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 1–4       | Arrays, strings, hash maps, two pointers, sliding window, stack/queue, binary search | 2–3 problems/week; explain time/space complexity aloud.                         |
| 5–8       | Linked lists, recursion/backtracking, intervals, tree traversal, BFS/DFS basics      | 3 problems/week; one timed 35-minute session.                                   |
| 9–12      | Heaps, graphs, topological sort, tries, common tree patterns                         | 3–4 problems/week; start mixed review.                                          |
| 13–16     | Dynamic programming fundamentals + mixed mediums                                     | 3–4 problems/week; focus on recognizing patterns, not volume.                   |
| 17–20     | Mixed medium set, weak-area repetition, interview communication                      | 4 problems/week; 2 timed sessions/month.                                        |
| 21–24     | Mock-style mixed questions                                                           | 2–3 quality problems/week + full mocks; prioritize explanation and correctness. |

A reasonable six-month target is roughly 55–75 well-reviewed problems, not hundreds of shallow completions. Re-solve missed problems after 3–7 days without reading the old solution.

## 13.2 SQL interview prep

- One timed SQL question per week from Weeks 2–12, then two per month for maintenance.
- Prioritize joins, aggregates, GROUP BY/HAVING, window functions, ranking, deduplication, top-N per group, and date/time grouping.
- Explain indexes and query plans conceptually; many senior full-stack interviews test data reasoning even when they do not ask advanced SQL syntax.

## 13.3 Full-stack knowledge drills

Once per week, choose one question and answer it in 5–10 minutes without notes. Then verify your answer and rewrite it in two paragraphs.

- What actually happens from typing a URL to rendering an authenticated React page?
- Explain Node’s event loop and what can block it.
- 401 vs 403 vs 409 vs 422: when would you use each?
- What problem does a database index solve, and what does it cost?
- How would you prevent duplicate payment/order creation after a client retry?
- Session cookie vs JWT: what trade-offs actually matter?
- What is server state in React, and why is it different from UI state?
- How would you diagnose an API whose p95 latency doubled over a week?
- What is eventual consistency? Give a realistic example.
- How would you safely roll out a risky backend change?

## 13.4 System design interview prep

Start in Week 9, after databases and Node fundamentals have begun to solidify. One 45-minute design every 1–2 weeks is enough initially.

| **Prompt**                               | **What to emphasize**                                                                     |
|------------------------------------------|-------------------------------------------------------------------------------------------|
| Design a file upload + processing system | Object storage, async jobs, retries, idempotency, status tracking, large files.           |
| Design a notification system             | Preferences, queueing, delivery retries, deduplication, provider failures, observability. |
| Design an inventory service              | Transactions, concurrency, stock ledger, consistency, indexing, auditability.             |
| Design a dashboard                       | Data freshness, aggregation, caching, pagination, frontend server state, performance.     |
| Design URL shortener / rate limiter      | Simple scalability building blocks, hashing/counters, storage, hot keys, trade-offs.      |

Use the same answer structure every time: clarify requirements → estimate rough scale → identify core entities/APIs → draw high-level design → choose storage → reason through failure/consistency → find bottlenecks → discuss alternatives/trade-offs.

## 13.5 Behavioral interview preparation from your actual career

Build 6–8 STAR(R) stories. Reuse a small set of strong projects rather than memorizing a separate story for every question.

| **Story source**                   | **Possible signals to prepare**                                                                                                        |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| NexTier / analytics portal         | Frontend architecture, code reviews, mentoring juniors, knowledge sharing, influencing quality.                                        |
| BNY Mellon pricing/liquidity app   | Sole frontend responsibility for a period, changing requirements, difficult delivery conditions, customer dissatisfaction, trade-offs. |
| Youse portal                       | Cross-functional work with UI/UX and Product; difficult production-only bug and debugging under uncertainty.                           |
| Spotify portals                    | Next.js, multilingual/CMS integration, client delivery, reusable components, responsive design.                                        |
| Next Mobile                        | Serverless Node/Express API, integrations, mobile/PWA context; useful full-stack ownership story.                                      |
| Current transition to Android + Go | Adaptability and learning a new stack; use only if you can state a concrete challenge/action/result.                                   |

For each story capture: Situation (2–3 sentences), Task/goal, your Actions (most of the answer), measurable Result if available, and Reflection—what you would repeat or change. Never invent impact numbers; qualitative outcomes are acceptable when metrics were not available.

## Resources — Interview track

- [Tech Interview Handbook: coding interview study plan](https://www.techinterviewhandbook.org/coding-interview-study-plan/) — A structured 3-month topic/practice plan; useful as a question bank and prioritization reference even on a longer schedule.
- [Tech Interview Handbook: DSA cheatsheets](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/) — Pattern-focused review of common interview data structures and algorithms.
- [Tech Interview Handbook: behavioral interviews](https://www.techinterviewhandbook.org/behavioral-interview/) — STAR(R), story selection, common behavioral themes, and mock-practice guidance.

# 14. Six checkpoints to keep the plan honest

| **Checkpoint** | **Evidence you should have**                                                                                         |
|----------------|----------------------------------------------------------------------------------------------------------------------|
| End Week 4     | Schema + migrations; raw SQL; N+1 reproduction/fix; EXPLAIN before/after; concurrency transaction test.              |
| End Week 8     | Node runtime labs for CPU blocking/workers, streams, graceful shutdown, and memory leak diagnosis.                   |
| End Week 12    | API contract/errors/pagination; idempotency test; async job with retries; authentication and authorization baseline. |
| End Week 16    | Threat model; security hardening; Dockerized stack; structured logs and correlation IDs.                             |
| End Week 20    | OpenTelemetry traces/metrics; k6 baseline + improvement; modern React server state; accessibility pass underway.     |
| End Week 24    | Polished portfolio release; ADRs and architecture diagrams; one full mock interview loop; story bank ready.          |

# 15. How to make the code useful in future applications

A portfolio project is valuable only if someone can understand it quickly. Treat documentation and reproducibility as part of the engineering output.

- README opening: what problem the project solves and why you built it.
- One architecture diagram; one data-model diagram. Keep them current.
- “Engineering highlights” section linking directly to: transaction/idempotency implementation, worker/retry logic, authorization tests, OpenTelemetry setup, k6 test, and performance investigation.
- One-command local setup where practical: Docker Compose + seeded development data.
- CI badge and a short test-strategy section.
- ADRs and “Known trade-offs / next steps.” Showing limits increases credibility.
- Keep commits small enough that interviewers can inspect meaningful changes.
- Do not include secrets, employer code, proprietary architecture, or copied production artifacts.

> **Best interview use**
> When an interviewer asks about transactions, caching, retries, performance, or observability, answer the concept first and then say: “I implemented this in a portfolio system; here was the trade-off and what I measured.” That is much stronger than citing a course.

# 17. Resource index

Prefer official documentation for implementation details and a small number of curated interview resources for practice. Links below were checked when this plan was prepared.

- [Node.js Learn](https://nodejs.org/learn) — Node runtime and backend fundamentals
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/18/tutorial.html) — SQL and relational fundamentals
- [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/18/sql-explain.html) — Query plans and performance
- [PostgreSQL Exercises](https://www.pgexercises.com/) — Hands-on SQL drills
- [OWASP WSTG](https://wstg.owasp.org/) — Web security testing
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) — Secure implementation guidance
- [OWASP Juice Shop](https://owasp.org/projects/juice-shop) — Hands-on security lab
- [MDN CORS security guide](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CORS) — Browser cross-origin security
- [Docker Node.js Guide](https://docs.docker.com/guides/nodejs/) — Containerizing Node apps
- [OpenTelemetry JS](https://opentelemetry.io/docs/languages/js/getting-started/) — Tracing and metrics
- [Grafana k6](https://grafana.com/docs/k6/latest/get-started/) — Load/performance testing
- [React Effects](https://react.dev/learn/synchronizing-with-effects) — Modern effects mental model
- [React: You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) — Avoid unnecessary effects
- [TanStack Query optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates) — Server state and optimistic UI
- [web.dev Accessibility](https://web.dev/accessibility) — Accessibility course/reference
- [Angular Signals](https://angular.dev/guide/signals) — Modern Angular reactivity
- [Angular Standalone](https://angular.dev/reference/migrations/standalone) — Standalone architecture
- [Angular Performance](https://angular.dev/best-practices/performance) — Profiling and performance
- [System Design Primer](https://github.com/donnemartin/system-design-primer) — System design fundamentals + interview practice
- [Tech Interview Handbook — Coding Plan](https://www.techinterviewhandbook.org/coding-interview-study-plan/) — Coding interview structure
- [Tech Interview Handbook — DSA](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/) — Algorithm patterns and practice
- [Tech Interview Handbook — System Design](https://www.techinterviewhandbook.org/system-design/) — System-design interview structure
- [Tech Interview Handbook — Behavioral](https://www.techinterviewhandbook.org/behavioral-interview/) — STAR(R) and story preparation

