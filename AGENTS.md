# PROJECT KNOWLEDGE BASE

**Generated:** 2026-01-12
**Commit:** d22f19ea218c

## OVERVIEW

Documentation/skill repository for Cloudflare platform—structured reference docs for AI/LLM consumption. No executable code.

## STRUCTURE

```
./
├── .jj/                          # VCS (jujutsu, NOT git)
├── .opencode/                    # OpenCode plugin config
├── docs/
│   └── products.md               # Platform overview (101 products)
└── skill/
    └── cloudflare/
        ├── SKILL.md              # Main skill manifest + decision trees
        ├── patterns.md           # Architecture patterns
        └── references/           # 60 product subdirs
            └── <product>/
                ├── README.md
                ├── api.md
                ├── configuration.md
                ├── patterns.md
                └── gotchas.md
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| Find a product | `skill/cloudflare/SKILL.md` | Decision trees + full index |
| Product reference | `skill/cloudflare/references/<product>/` | 5-file structure |
| Common patterns | `skill/cloudflare/patterns.md` | Multi-product architectures |
| Platform overview | `docs/products.md` | All 101 products listed |

## CONVENTIONS

### VCS: jujutsu (NOT git)

```bash
# CORRECT
jj status
jj log
jj new
jj commit -m "msg"

# WRONG - do not use
git status  # .jj/ present = use jj
```

### Reference File Structure

Every product follows 5-file pattern:
- `README.md` — Overview, when to use
- `api.md` — Runtime API reference
- `configuration.md` — `wrangler.toml` + binding setup
- `patterns.md` — Usage patterns
- `gotchas.md` — Pitfalls, limitations

Some products use monolithic `SKILL.md` instead (migration in progress): `ai-gateway`, `c3`, `email-workers`, `pipelines`, `r2-sql`, `tail-workers`, `turn`, `vectorize`, `workers-ai`, `workers-vpc`, `zaraz`

### YAML Frontmatter

`SKILL.md` files use frontmatter for machine parsing:
```yaml
---
name: product-name
description: Brief description
references:
  - related-product
---
```

## ANTI-PATTERNS

### SQL Injection (D1)

```typescript
// NEVER - string interpolation
const result = await db.prepare(`SELECT * FROM users WHERE id = ${id}`).all();

// ALWAYS - prepared statements with bind()
const result = await db.prepare("SELECT * FROM users WHERE id = ?").bind(id).all();
```

### Secrets Management

```bash
# NEVER commit .dev.vars (contains secrets)
# NEVER hardcode secrets in code
```

### Resource Management

```typescript
// ALWAYS close browser in finally block (browser-rendering)
const browser = await puppeteer.launch();
try {
  // ...
} finally {
  await browser.close();
}

// ALWAYS call proxyToSandbox() first (sandbox)
await proxyToSandbox();
```

### Configuration

```toml
# ALWAYS set compatibility_date for new projects (workers)
compatibility_date = "2026-01-12"
```

### Bot Management

JSD (JavaScript Detection) cannot be used on first page visit—requires HTML page loaded first.

## NOTES

- No CI/CD configured (docs-only repo)
- No linting/formatting (no code to lint)
- `.opencode/` contains plugin config, not project code
- Two index locations exist: `SKILL.md` (primary) vs `docs/products.md` (overview)
