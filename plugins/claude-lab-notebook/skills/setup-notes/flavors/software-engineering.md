# Software Engineering Flavor

Software development spikes, architecture decisions, and technical troubleshooting.

## Configuration

```yaml
name: software-engineering
display_name: Software Engineering
description: Feature spikes, architecture decisions, and technical documentation
```

## Directory Structure

```yaml
directories:
  - spikes          # Time-boxed explorations, proof of concepts
  - decisions       # Architecture decisions, technology choices
  - troubleshooting # Error patterns, debugging guides
  - architecture    # System design, component diagrams
  - research        # Libraries, frameworks, external references
```

## Protocol Examples

Use these domain-specific examples in the note-taking protocol.

### When to Load Additional Files

**User says:** "I'm working on the caching layer"
**You respond:** "I see from INDEX.md we explored caching strategies on 2025-01-10. Should I read `spikes/2025-01-10-redis-caching.md` for details?"

**User says:** "I'm getting a TypeScript error"
**You respond:** "Should I check `troubleshooting/typescript-errors.md` for known solutions?"

**User says:** "Why did we choose this API design?"
**You respond:** "Should I read `decisions/2025-01-12-api-versioning.md` for the rationale?"

**User says:** "How does the auth system work?"
**You respond:** "Should I read `architecture/auth-system.md` for the system design?"

### Progressive Disclosure Example Session

```
[Session starts]
Claude: [Reads notes/INDEX.md - 1K tokens]
Claude: [Reads notes/quick-reference.md - 500 tokens]
Claude: "I've loaded your project notes index. What are you working on?"

User: "I want to add rate limiting to the API"

Claude: "From INDEX.md, I see we explored API middleware on 2025-01-09
and Redis integration on 2025-01-11. Should I read those spikes?"

User: "Yes, read the Redis one"

Claude: [Uses view on notes/spikes/2025-01-11-redis-integration.md - 2K tokens]
Claude: "Based on that spike, we have Redis running on port 6379.
The connection pooling is already set up. Want to build on that?"

[Works on task...]

[Session ends]
Claude: "Should I create a spike note for today's work?"

User: "Yes"

Claude: [Creates notes/spikes/2025-01-15-rate-limiting.md]
Claude: "The spike note has been created. Should I update INDEX.md
since we found sliding window rate limiting works best?"

Total context used: ~5K tokens instead of 50K+ if all files loaded
```

## INDEX.md Examples

```markdown
## All Spikes
<!-- Each entry has: Title, Tags, Finding, File link -->
<!-- Example:
**2025-01-15: Redis caching implementation**
- Tags: redis, caching, performance, api
- Finding: Redis with 5-minute TTL reduces DB load by 60%
- File: [spikes/2025-01-15-redis-caching.md](spikes/2025-01-15-redis-caching.md)
-->

## All Decisions
<!-- Example:
**2025-01-14: Choose REST over GraphQL**
- Tags: api, rest, graphql, architecture
- Finding: REST chosen for simplicity and team familiarity
- File: [decisions/2025-01-14-api-style.md](decisions/2025-01-14-api-style.md)
-->

## All Troubleshooting Guides
<!-- Example:
**TypeScript strict mode errors**
- Tags: typescript, strict, type-errors, build
- Finding: Use `as const` for literal inference, avoid `any`
- File: [troubleshooting/typescript-strict-errors.md](troubleshooting/typescript-strict-errors.md)
-->

## All Architecture
<!-- Example:
**Authentication system**
- Tags: auth, jwt, security, middleware
- Finding: JWT with refresh tokens, 15-min access expiry
- File: [architecture/auth-system.md](architecture/auth-system.md)
-->

## All Research
<!-- Example:
**State management libraries**
- Tags: state, redux, zustand, jotai
- Finding: Zustand best for our use case (simple API, good DX)
- File: [research/state-management.md](research/state-management.md)
-->
```

## quick-reference.md Format

```yaml
# Configuration format for "What's Working Right Now"
format: env_vars
example: |
  NODE_ENV=development
  DATABASE_URL=postgresql://localhost:5432/app
  REDIS_URL=redis://localhost:6379
  JWT_SECRET=dev-secret-change-in-prod

  # Dependencies
  node: 20.x
  pnpm: 8.x
  postgres: 15
  redis: 7
```

## Tool Integration

- **CI/CD**: GitHub Actions, CircleCI, Jenkins
- **Containers**: Docker, docker-compose, Kubernetes
- **Testing**: Jest, Vitest, Playwright, Cypress
- **Monitoring**: Datadog, Sentry, Prometheus/Grafana
- **API Docs**: OpenAPI/Swagger, Postman
