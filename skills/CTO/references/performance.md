# Performance Audit — CTO Reference

## When to Load
Load this reference when:
- The user asks about performance specifically (`/CTO performance`)
- You notice potential bottlenecks in the codebase
- The project handles large datasets, real-time updates, or media
- Load times, responsiveness, or resource usage are concerns

## Performance Evaluation Framework

### Rendering & UI Performance
- 60fps target for animations and transitions (90fps for visionOS)
- No layout thrashing or unnecessary re-renders
- Images and media are appropriately sized and lazy-loaded
- Critical rendering path is optimized (minimal blocking resources)
- Lists virtualize large datasets (not rendering 10,000 DOM nodes)

### Network Performance
- API calls are batched where possible
- Appropriate caching strategy (HTTP cache, local cache, stale-while-revalidate)
- Payload sizes are minimal (no over-fetching)
- Compression enabled (gzip/brotli)
- Prefetching for predictable navigation patterns

### Data & Computation
- Database queries are indexed and efficient (no N+1 queries)
- Heavy computation offloaded (background threads, web workers, async)
- Memory usage is bounded (no leaks, no unbounded growth)
- Pagination for large result sets

### App Startup
- Cold start time is acceptable for the platform
- Lazy loading for non-critical modules
- Critical path loads first, secondary content deferred
- Bundle size is monitored and reasonable

### Platform-Specific (Apple)
- Instruments profiling for CPU, memory, energy
- Core Data / SwiftData fetch requests are optimized
- Background tasks use appropriate APIs (BGTaskScheduler)
- Energy impact is considered (no unnecessary wake-ups)
- Metal performance for GPU-intensive work

### Measurement
- Performance budgets defined (load time, bundle size, FCP, LCP)
- Real user monitoring or synthetic testing in place
- Performance regression detection in CI

## Review Output Format
When delivering a performance-focused review, add a **Performance Assessment** section:

```markdown
### Performance Assessment
- **Overall:** [Fast / Acceptable / Sluggish / Problematic]
- **Key Metrics:** [Load time, bundle size, FPS, etc. — whatever applies]
- **Bottlenecks:**
  1. **[Bottleneck]** — [What's slow and why]
     - Impact: [User-perceived effect]
     - Fix: [Specific optimization]
```
