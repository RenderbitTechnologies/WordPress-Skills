# WordPress Skills

A collection of AI agent skills for managing WordPress sites via WP-CLI over SSH.

These skills are designed to be installed in [Claude.ai](https://claude.ai) or other AI agents and give agents structured, opinionated workflows for common WordPress content management tasks — without needing a plugin, REST API integration, or admin panel access.

---

## Skills

### [`wp-cli-posts`](./wp-cli-posts/)

Create, update, and manage WordPress posts, pages, and custom post types using WP-CLI over SSH.

**What it does:**

- Creates and updates posts, pages, and custom post types (CPTs)
- Uploads media via WP-CLI and injects attachment URLs into post content
- Fetches existing content before applying changes (read → patch → verify)
- Verifies the published URL after every update for discrepancies
- Supports WP Multisite — both subfolder and subdomain setups
- Guides SSH/WP-CLI configuration if not already set up

**Guardrails built in:**

- Always asks for the page URL before any update
- Never guesses the post type — asks explicitly if it can't be inferred
- Makes the minimum requested change only — no unsolicited edits
- Asks which sub-site to target for multisite create tasks

### [`wordpress-optimization`](./wordpress-optimization/)

WordPress performance optimization — speed testing, caching, image optimization, JS/CSS delivery, database tuning, and Core Web Vitals.

**What it does:**

- Achieves 90+ mobile PageSpeed scores with a repeatable methodology
- Covers caching (WP Rocket, Flyingpress), image optimization, JS delay, and unused CSS removal
- Addresses page builders (Elementor, Divi, WPBakery, Beaver, Bricks) and theme-specific tuning
- Includes TTFB optimization, database cleanup, and security hardening
- Targets Core Web Vitals (LCP, CLS, INP) using Debug Bear for diagnosis and PSI for verification

**Guardrails built in:**

- Always tests mobile scores only — desktop is explicitly excluded
- Recommends averaging 3 tests to account for inter-test variance
- Uses Debug Bear for diagnosis, PageSpeed Insights for verification — never optimizes blindly
- Prioritizes free/built-in solutions before recommending paid plugins

---

## Installation

1. Download the `.skill` file for the skill you want from the [Releases](../../releases) page (or directly from the skill's directory in this repo).
2. In Claude.ai, go to **Settings → Skills**.
3. Upload the `.skill` file.
4. Claude will now automatically use the skill when relevant.

---

## Prerequisites

Each skill assumes the following are in place on your local machine:

- **WP-CLI** installed locally (or accessible on the remote server)
- **SSH access** to the remote WordPress host
- A `~/.wp-cli/config.yml` with a named alias for the remote server

Minimal `config.yml` example:

```yaml
@production:
  ssh: user@your-server.com
  path: /var/www/html
```

For AWS Elastic Beanstalk with a key pair:

```yaml
@production:
  ssh: ec2-user@your-eb-hostname.amazonaws.com --ssh-keys=~/.ssh/your-key.pem
  path: /var/www/html
```

If SSH isn't configured, the skill will detect this and walk you through the setup before proceeding.

---

## Contributing

Skills follow a standard structure:

```
skill-name/
├── SKILL.md          # Required — skill instructions and metadata
└── references/       # Optional — additional reference docs
```

To add a new skill, open a PR with your `SKILL.md` in a new directory at the project root. Please include a description of what the skill does and when it should trigger in the YAML frontmatter.

---

## Maintained by

[Renderbit Technologies](https://github.com/RenderbitTechnologies)
