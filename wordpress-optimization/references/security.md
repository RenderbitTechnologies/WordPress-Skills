# Security — Reference

## Performance-Conscious Security Architecture

**3 firewall layers (all complementary):**
1. **WAF** — Cloudflare WAF, handles threats at network edge before reaching server
2. **Server-level firewall** — 7G (NGINX) or 8G (Apache) from Jeff Starr
3. **Application-level firewall** — NinjaFirewall or BBQ Firewall (WordPress)

---

## Firewall Plugins

### WAFs (Network Level)
| Plugin/Service | Cost | Notes |
|----------------|------|-------|
| Cloudflare WAF | Free/Pro | Best free WAF; combine with Argo for best results |
| BunkerWeb | Free | Flexible, configurable server-level WAF |
| NGINX App Protect WAF | Free | Outperforms cloud WAFs in GigaOm tests |

Cloudflare firewall rules for WordPress: https://gridpane.com/blog/cloudflare-firewall-rules-for-securing-wordpress-websites/

### Server-Level Firewalls
| Plugin | Cost | Notes |
|--------|------|-------|
| 7G NGINX Firewall | Free | https://perishablepress.com/7g-firewall-nginx/ |
| 8G Apache Firewall | Free | https://perishablepress.com/8g-firewall/ |

### Application-Level (WordPress)
| Plugin | Cost | Notes |
|--------|------|-------|
| NinjaFirewall | Free/Pro | Lightweight; recommended |
| BBQ Firewall | Free | From Jeff Starr; pair with 8G |

---

## DO NOT Use Wordfence on Live Sites

Wordfence costs **10–15 PageSpeed points** on mobile.

**Use instead:**
- Scan for malware: GOTMLS (Free), or Wordfence on **staging only**
- Security headers: Headers Security Advanced & HSTS WP (Free)
- Vulnerability patching: **Patchstack (Freemium)** — applies virtual patches before dev updates

---

## Brute Force Protection

### Limit Login Attempts Reloaded (Free)
https://wordpress.org/plugins/limit-login-attempts-reloaded/
Limits login attempts on standard, XMLRPC, WooCommerce, and custom login pages.

---

## XML-RPC

**Disable it unless a plugin explicitly requires it.** XML-RPC enables DDoS amplification (pingbacks) and brute-force attacks.

| Plugin | Cost |
|--------|------|
| Asset Cleanup (has XML-RPC toggle) | Free/Pro |
| Simple Disable XML-RPC | Free |

---

## Two-Factor Authentication

| Plugin | Cost | Notes |
|--------|------|-------|
| Two Factor | Free | Official WP core dev team plugin |
| Fluent Auth | Free | Lightweight 2FA |

---

## Security Headers

Your site should score **A** on https://securityheaders.com/

**Headers Security Advanced & HSTS WP (Free)**: https://wordpress.org/plugins/headers-security-advanced-hsts-wp/
Automatically implements all security header best practices.

---

## Block Bad Bots and AI Scrapers

| Plugin | Cost |
|--------|------|
| Banhammer | Free |
| Block AI Crawlers | Free |
| VigIA | Free |
| Dark Visitors | Free |
| Website LLMs.txt | Free |

---

## PHP Error Monitoring

PHP errors can significantly impact performance. Monitor and fix them.

| Plugin | Cost | Notes |
|--------|------|-------|
| Debug Log Manager | Free | Enable/view/clear PHP debug log |
| Query Monitor | Free | Reports PHP errors |

---

## Audit Logging

| Plugin | Cost | Notes |
|--------|------|-------|
| CoreActivity Log | Free | Most comprehensive; 28 components, 174 events |
| Stream | Free | User/system actions; email alerts + webhooks |
| Simple History | Free | Detailed audit log with plugin integrations |
| Aryo Activity Log | Free | Activity monitoring |

---

## Malware Scanning (Staging Only)

| Plugin | Cost | Notes |
|--------|------|-------|
| GOTMLS | Free | Lightweight anti-malware |
| Wordfence | Free | Good scanner; **staging only** |

---

## Additional Security Tools

| Tool | Cost | Notes |
|------|------|-------|
| Patchstack | Freemium | Virtual patches; highly recommended |
| Hardenize | Free | Scan for common security issues |
| No Unsafe Inline Scripts | Free | CSP enforcement |
| Imunify360 | Paid | Comprehensive security suite |
| Website File Changes Monitor | Free | Email alerts on file changes |

---

## Server Hardening

- Linux Server Hardening Guide: https://ivansalloum.com/comprehensive-linux-server-hardening-guide/
- SOC Compliance for WordPress: https://www.kevinleary.net/blog/soc-compliance-wordpress-websites/
- Kevin Leary security guide: https://www.kevinleary.net/blog/securing-a-wordpress-website/

---

## Automatic Server Software Updates

Outdated PHP/MySQL/NGINX versions = security vulnerabilities.
Automate via system cron jobs.

Cron generators:
- https://crontab.cronhub.io/
- https://crontab.guru/
- https://crontab-generator.org/
