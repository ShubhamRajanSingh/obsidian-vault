# SDE 2 Interview Preparation Plan

#tags: #dsa #systemdesign #lld #hld #interview-prep #tracker

## Goal and operating rules

Prepare for SDE 2 backend interviews with deliberate practice across DSA, LLD, HLD, resume/project discussions, and core CS. Prior notes are a catalogue of past practice, **not evidence of current mastery**. Every topic starts as unverified and is assessed through recall and fresh solving.

- Daily target: **2 DSA problems (Medium/Hard)** plus **1 LLD or HLD prompt**.
- Use Java for interview DSA solutions unless a prompt explicitly calls for another language.
- For each DSA problem: clarify constraints, state a brute-force baseline, derive the optimized approach, analyze time/space, code, test edge cases, and explain aloud. Start without reading the existing solution.
- A prior vault note means “available for spaced review,” not “done.” Mark complete only after solving/explaining without relying on the note; record hints, time, and confidence.
- At the 9:00 PM IST check-in, report `done`, `partial`, or `not started` for each task, plus blockers and confidence. If no update arrives before the next morning, mark the day unreported and carry its unfinished tasks forward. Do not silently mark work complete.
- To avoid an unbounded backlog, the morning plan prioritizes carried work; new tasks resume after the carryover is cleared or explicitly rescheduled with the user.
- Revisit completed topics after 1, 3, and 7 days; lower the priority only after successful unaided recall.

## Resume-specific preparation

Build crisp STAR-style stories and technical deep dives for:

1. Redis Streams task execution: producers, consumer groups, retries/failure recovery, 12,000 tasks/min on a 2 GB pod, scaling and bottlenecks.
2. Multi-tenancy and database separation: tenant isolation, onboarding, migrations, operational trade-offs.
3. DB-layer AES-256 PII encryption: key management, rotation, nonce/IV handling, access control, query/index implications, threat model.
4. ZSTD dictionary compression: workload fit, dictionary lifecycle, CPU/latency/storage trade-offs, measured 30–40% reduction.
5. Rules and Approval DSLs: grammar/model, evaluation, extensibility, validation, safety, workflow semantics.
6. Gateway error mapping and recovery: retry/reroute/webhook decisions, idempotency, failure modes, reported 10% SR improvement and attribution.
7. Payment provider integrations, accounts payable, virtual cards, integrity verification, and cross-product IAM: architecture, consistency, security, and ownership boundaries.
8. Leadership/behavioral: a difficult incident, disagreement, trade-off, missed expectation, mentoring/collaboration, and measurable impact. Substantiate every metric and distinguish personal contribution from team outcomes.

## 12-week curriculum (adapt pace to check-ins)

| Weeks | DSA focus | Design / other focus |
|---|---|---|
| 1–2 | Arrays, hashing, two pointers, sliding window, binary search, intervals | LLD foundations: OOP, SOLID, composition, UML, APIs; parking lot and rate limiter |
| 3–4 | Linked lists, stacks/queues, heaps, trees/BST, tries | LLD: cache/LRU, elevator, booking/workflow; concurrency, synchronization, Java collections |
| 5–6 | Graph traversal, topological sort, union-find, shortest paths, backtracking/greedy | HLD foundations: requirements, capacity estimates, APIs, schema, partitioning; URL shortener and notification service |
| 7–8 | DP (1D/2D, knapsack, sequence/string), monotonic stack, mixed review | HLD: feeds, chat, payments/ledger, caching, queues, consistency, sharding, reliability |
| 9–10 | Timed mixed Medium/Hard sets; revisit weak patterns | Mock LLD/HLD interviews; Redis, Kafka/queues, SQL/indexing/transactions, networking, OS/concurrency |
| 11–12 | Company-style timed rounds, error-log revision, spaced repetition | Full interview loops; resume deep-dives, behavioral stories, negotiation/job-search readiness |

Keep the daily design question alternating between LLD and HLD by default, while giving extra practice to the weaker area discovered in mocks. Reserve one weekly session for a mock interview/review rather than adding a fourth daily deliverable.

## Day 1 — 2026-10-16 (baseline; nothing presumed mastered)

- [ ] DSA Medium — [[Longest Substring Without Repeating Characters]]: solve unaided; explain the sliding-window invariant and complexity.
- [ ] DSA Hard — [[Trapping Rainwater]]: compare two pointers with prefix/suffix maxima; test edge cases and explain the invariant.
- [ ] LLD — Design a parking lot: requirements, entities, interfaces, allocation strategy, concurrency, extensibility, and tests.
- [ ] Resume drill — prepare a 2-minute overview of the Redis Streams system and list assumptions/metrics to validate.

## Rolling tracker

| Date (IST) | Task | Status | Time / confidence | Blocker, mistake, or next review |
|---|---|---|---|---|
| 2026-10-16 | Longest Substring Without Repeating Characters | Not started | — | — |
| 2026-10-16 | Trapping Rainwater | Not started | — | — |
| 2026-10-16 | Parking Lot LLD | Not started | — | — |
| 2026-10-16 | Redis Streams resume drill | Not started | — | — |

## Weekly review checklist

- [ ] Count completed, partial, carried, and skipped tasks; identify the reason for each miss without assuming mastery.
- [ ] Pick the three weakest patterns/design areas from evidence and schedule them for focused review.
- [ ] Complete one timed DSA mock and one LLD/HLD mock or equivalent aloud walkthrough.
- [ ] Refine one resume story with concrete scope, personal ownership, trade-offs, and defensible metrics.
- [ ] Adjust the next week's mix based on performance and available time.

## Existing vault material to revisit

- [[Graph Algorithms]] and [[DSA Revision Dashboard]] are useful starting points, not a completed-topic checklist.
- DSA questions are catalogued in `Knowledge/DSA/Questions/`; revisit selected notes only after an unaided attempt.
- Existing design plans: [[LLD_SDE2_Study_Plan]], [[HLD_SDE2_Study_Plan]], [[HLD-System-Design-Study-Map]], [[UML Class Diagrams]], [[Redis Internals & Use Cases]], [[Caching]], and [[Message Queue]].
- The resume's payment, security, database, and distributed-systems examples should drive project-specific follow-ups alongside these general topics.

## Automation contract

- Morning post: publish the day's two Medium/Hard DSA tasks, one design prompt, and any carryover; include links to this tracker.
- Night post: request a concise status update for each item and any blockers.
- Update this note based only on explicit user check-ins or clear evidence from the check-in channel. When no check-in has arrived by the next morning, mark remaining items `Deferred — no update` and carry them forward.
- All scheduled times are Asia/Kolkata (IST). Current proposed cadence: 8:00 AM and 9:00 PM daily.

## Job search and company-specific preparation

- Search active India-based or Remote-India roles at SDE 2 / Backend Engineer II / mid-level backend/platform levels that match my payment infrastructure, distributed systems, Redis Streams, database, security, APIs, Java/Haskell experience (roughly 3–6 years).
- Hard compensation floor: **₹40 lakh annual base pay**, excluding bonus, equity, joining bonus, and other variable pay. If the job description omits salary, research role-, level-, and India-location-specific base pay from credible sources. Keep base and total compensation separate; include a researched estimate only when evidence supports the floor, state confidence and cite sources. Exclude roles where base pay cannot be verified as meeting the floor.
- Post only currently open, not previously posted roles in Discord `#jobs`; assign each a stable ID (`JOB-YYYYMMDD-NN`) and include employer application link, role/location, experience fit, base-pay evidence, and sources.
- To mark one for follow-up, reply in `#jobs` with `Interested: JOB-...` (include the exact ID). Research interview reports/questions for that company and role, cite sources, label anecdotal candidate reports as such, and add targeted practice items to the company-specific queue below.
- The morning planner should incorporate active company-specific questions into the existing daily workload, replacing or adapting tasks rather than adding an unbounded extra load.

### Interested-job interview prep queue

| Job ID | Company / role | Research date | Reported question themes (with sources) | Prep status |
|---|---|---|---|---|
| — | — | — | — | — |

### Posted job IDs

| Job ID | Company / role | Location | Base-pay evidence and confidence | Posting date | Status |
|---|---|---|---|---|---|
| — | — | — | — | — | — |

### Leads collected during the day (pending nightly digest)

Collect verified, qualifying openings here as they are found; the 11:59 PM IST scout rechecks availability, deduplicates, posts new qualifying leads to `#jobs`, then changes their status to `Posted`.

| Job ID | Company / role | Location | Application URL | Base-pay evidence, source, confidence | Collected (IST) | Status |
|---|---|---|---|---|---|---|
| JOB-20261019-01 | Amazon — Software Development Engineer II (Amazon Business pricing platform) | Hyderabad, India | https://www.amazon.jobs/applicant/jobs/10555781/apply | Posting requires 3+ years and backend software development; Java and distributed-systems experience align. Estimated base ₹41–49 LPA: Levels.fyi SDE II Hyderabad average base ₹48.56 LPA; Glassdoor Hyderabad SDE-2 base submissions include ₹41 LPA for 4–6 years [2][3]. Employer posting omits salary; medium confidence. Salary sources: [2] https://www.levels.fyi/companies/amazon/salaries/software-engineer/levels/sde-ii/locations/greater-hyderabad-area ; [3] https://www.glassdoor.co.in/Salary/Amazon-SDE-2-Hyderabad-India-Salaries-E6036_DAO.htm?filter.jobTitleExact=SDE-2&selectedLocationString=M%2C1076 | 2026-10-19 13:04 IST | Pending nightly digest |
