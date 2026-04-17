# Performance Template

For profiling and optimization work. Forces measurement before and after changes, explicit
target metrics, and validation that behavioral correctness is preserved. Prevents premature
optimization by requiring evidence of the bottleneck before touching code.

```md
# Performance Session

## Symptom
[What is slow, heavy, or failing under load? Be specific — "API response takes 4s" not "it's slow".]

## Context
- Repo: [name + path]
- Language: [Python / Node / Rust / Go / etc.]
- Framework: [FastAPI / Django / Express / Next.js / etc.]
- Deployment: [Fly.io / Vercel / AWS / local]
- Component: [endpoint / page / background job / query / startup / build]
- Traffic pattern: [steady / bursty / growing / one-time batch]

## Performance Target
Define what "fixed" means. At least one must be quantified.

| Metric | Current | Target | Hard Limit |
|--------|---------|--------|------------|
| Latency (p50) | [ms] | [ms] | [ms] |
| Latency (p99) | [ms] | [ms] | [ms] |
| Throughput | [req/s or items/s] | [req/s] | [min acceptable] |
| Memory usage | [MB] | [MB] | [MB — OOM threshold] |
| CPU usage | [%] | [%] | [%] |
| Startup time | [s] | [s] | [s] |
| Bundle size | [KB] | [KB] | [KB] |

## Measurement Baseline
Capture these BEFORE making any changes.

### Environment
- Hardware: [CPU cores, RAM, disk type]
- OS: [version]
- Runtime version: [Python 3.12 / Node 22 / etc.]
- Database: [version, connection pool size, data volume]
- Concurrency: [workers, threads, async event loop]

### Baseline Measurements
- Tool used: [ab / wrk / k6 / locust / time / cProfile / py-spy / perf / Chrome DevTools]
- Load parameters: [concurrent users, duration, request pattern]
- Results:
  ```
  [paste raw benchmark output here]
  ```
- Flamegraph or profile: [path to saved profile or screenshot]

## Suspected Bottleneck
[Where do you think the problem is? This is a hypothesis, not a conclusion.]

Possible categories:
- [ ] N+1 queries or missing indexes
- [ ] Synchronous I/O blocking async event loop
- [ ] Unbounded memory allocation (large lists, no streaming)
- [ ] CPU-bound computation in hot path
- [ ] Excessive serialization/deserialization
- [ ] Missing caching (repeated expensive computation)
- [ ] Network round-trips (chatty service calls)
- [ ] Lock contention or GIL saturation
- [ ] Large payload sizes (over-fetching data)
- [ ] Cold start / import time
- [ ] Frontend: layout thrashing, unoptimized images, blocking scripts

## Profiling Approach
1. [Tool and method — e.g., "cProfile on the /api/search endpoint with 1000 rows"]
2. [What to look for — e.g., "functions consuming >10% of total time"]
3. [How to isolate — e.g., "disable caching to measure raw query time"]

### Profiling Commands
```bash
# [tool-specific commands — adapt to your stack]
# Python: python -m cProfile -o profile.out app.py
# Python (sampling): py-spy record -o profile.svg -- python app.py
# Node: node --prof app.js
# HTTP load: wrk -t4 -c100 -d30s http://localhost:8000/endpoint
# SQLite: EXPLAIN QUERY PLAN SELECT ...
```

## Optimization Plan
After profiling confirms the bottleneck, document the fix strategy.

| Change | Expected Impact | Risk |
|--------|----------------|------|
| [description] | [estimated improvement] | [what could break] |

## Constraints
- No behavioral changes — output must remain identical for same input
- Measure before AND after every change (no "it feels faster")
- One change at a time — isolate the impact of each optimization
- Do not optimize code that is not in the hot path
- Readability tradeoffs must be justified by measured gains
- Do not add caching without defining invalidation strategy
- Do not change public API signatures for performance

## Validation

### Correctness
- [ ] All existing tests pass after optimization
- [ ] Edge cases tested (empty input, large input, concurrent access)
- [ ] Output compared against baseline for identical inputs

### Performance
- [ ] Same benchmark repeated with same parameters
- [ ] Improvement measured and documented (not estimated)
- [ ] No regression in other metrics (fixing latency did not spike memory)
- [ ] Results are reproducible (run benchmark 3+ times, report median)

### After Measurements
- Tool used: [same as baseline]
- Load parameters: [same as baseline]
- Results:
  ```
  [paste raw benchmark output here]
  ```

## Required Output
1. Root cause (what was actually slow, with profiling evidence)
2. Optimization applied (what changed, why this approach)
3. Before/after comparison table (same metrics, same conditions)
4. Correctness validation (tests pass, output unchanged)
5. Trade-offs acknowledged (readability, complexity, maintenance cost)
6. Follow-up items (further optimization opportunities not addressed this session)
```
