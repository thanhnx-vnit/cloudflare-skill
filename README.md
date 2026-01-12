# Cloudflare Skill for OpenCode

Comprehensive Cloudflare platform reference docs for AI/LLM consumption. Covers Workers, Pages, storage (KV, D1, R2), AI (Workers AI, Vectorize, Agents SDK), networking, security, and infrastructure-as-code.

## Install

Clone into your project's `.opencode/skill/` directory:

```bash
git clone https://github.com/YOUR_ORG/cloudflare-skill.git /tmp/cf-skill && \
  cp -r /tmp/cf-skill/skill/cloudflare .opencode/skill/cloudflare && \
  rm -rf /tmp/cf-skill
```

Or for global installation (available in all projects):

```bash
git clone https://github.com/YOUR_ORG/cloudflare-skill.git /tmp/cf-skill && \
  cp -r /tmp/cf-skill/skill/cloudflare ~/.config/opencode/skill/cloudflare && \
  rm -rf /tmp/cf-skill
```

> **Note:** Replace `YOUR_ORG` with the actual GitHub org/user.

## Usage

Once installed, the skill appears in OpenCode's `<available_skills>` list. The agent loads it automatically when working on Cloudflare tasks, or you can request it explicitly:

```
Load the cloudflare skill and help me set up a D1 database
```

## Structure

```
skill/cloudflare/
├── SKILL.md              # Main manifest + decision trees
├── patterns.md           # Multi-product architecture patterns
└── references/           # 60 product subdirectories
    └── <product>/
        ├── README.md         # Overview, when to use
        ├── api.md            # Runtime API reference
        ├── configuration.md  # wrangler.toml + bindings
        ├── patterns.md       # Usage patterns
        └── gotchas.md        # Pitfalls, limitations
```

### Decision Trees

The main `SKILL.md` contains decision trees for:
- Running code (Workers, Pages, Durable Objects, Workflows, Containers)
- Storage (KV, D1, R2, Queues, Vectorize)
- AI/ML (Workers AI, Vectorize, Agents SDK, AI Gateway)
- Networking (Tunnel, Spectrum, WebRTC)
- Security (WAF, DDoS, Bot Management, Turnstile)
- Media (Images, Stream, Browser Rendering)
- Infrastructure-as-code (Terraform, Pulumi)

## Products Covered

Workers, Pages, D1, Durable Objects, KV, R2, Queues, Hyperdrive, Workers AI, Vectorize, Agents SDK, AI Gateway, Tunnel, Spectrum, WAF, DDoS, Bot Management, Turnstile, Images, Stream, Browser Rendering, Terraform, Pulumi, and 40+ more.

## License

MIT - see [LICENSE](LICENSE)
