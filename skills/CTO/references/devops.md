# DevOps & Infrastructure — CTO Reference

## When to Load
Load this reference when:
- The user asks about deployment, CI/CD, or infrastructure (`/CTO devops`)
- The project has deployment configurations, Dockerfiles, or CI pipelines
- Questions about environments, scaling, monitoring, or reliability arise
- Build and release processes need evaluation

## DevOps Evaluation Framework

### Build & CI/CD
- Build process is reproducible and deterministic
- CI pipeline catches errors before merge (lint, test, type-check)
- Build times are reasonable — no unnecessary steps
- Artifacts are versioned and traceable to commits

### Deployment
- Deployment is automated and repeatable
- Rollback strategy exists and is tested
- Environment parity (dev ≈ staging ≈ production)
- Zero-downtime deployment where appropriate
- Infrastructure as code (not manual console clicks)

### Environments & Configuration
- Environment-specific config is externalized (not in code)
- Secrets are injected, not committed
- Feature flags where needed for staged rollouts
- Clear separation between environments

### Observability
- Logging is structured and queryable
- Errors are captured and alerted on (not silently swallowed)
- Key metrics are tracked (latency, error rate, throughput)
- Health checks exist for critical services

### Reliability
- Graceful degradation under load
- Database backups and recovery tested
- Rate limiting and circuit breakers where appropriate
- Dependency failure handling (what happens when an external API goes down?)

### Developer Experience
- Local development setup is documented and fast
- `README` or onboarding gets a new dev running in minutes
- Consistent tooling (linters, formatters, pre-commit hooks)
- Clear branching and release strategy

### iOS / TestFlight Deployment
The `/testflight` skill handles the full iOS deployment pipeline — from source code to TestFlight. When evaluating iOS projects:
- Signing certs should be in the **System keychain** (enables SSH/headless builds)
- App Store Connect API key (`.p8`) should be at `~/.appstoreconnect/private_keys/`
- Apple ID credentials should be in the System keychain (loaded via `~/.zshrc` env vars)
- Pipeline: `xcodebuild archive` → `xcodebuild -exportArchive` → `eas submit`
- EAS handles app creation in Connect (Apple's API doesn't support it)
- Never manipulate keychains or revoke certs programmatically — if signing breaks, flag it

## Review Output Format
When delivering a devops-focused review, add an **Infrastructure Assessment** section:

```markdown
### Infrastructure Assessment
- **Deployment Maturity:** [Manual / Semi-automated / Fully automated]
- **Observability:** [Blind / Basic logging / Monitored / Fully observable]
- **Findings:**
  1. **[Finding]** — [Operational risk]
     - Impact: [What could go wrong]
     - Fix: [Specific action]
```
