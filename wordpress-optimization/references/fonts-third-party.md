# Fonts & Third-Party Files — Reference

## Font Best Practices

1. **Use WOFF2 only** — best compression; widest browser support
2. **Host fonts locally** — never load from Google Fonts CDN (external = latency)
3. **Subset fonts** — strip unused characters to reduce file size by 50–90%
4. **Preload above-fold fonts** — preload attribute on fonts used in viewport
5. **Replace icon font libraries with SVGs** — Font Awesome loads 90+ KB; use individual SVGs instead
6. **Use font-display: swap** — prevents invisible text while font loads

---

## Locally Host Google Fonts

### Manual download
**Google Fonts Web Helper**: https://gwfh.mranftl.com/fonts
Download WOFF2 files → upload to WordPress → select in builder.

### Automatic plugin solutions
| Plugin | Cost | Notes |
|--------|------|-------|
| Yabe Webfont | Free | Best; GDPR-friendly; most flexible |
| OMGF | Free | Solid alternative |
| Flyingpress | Paid | Can auto-host; cannot serve manually uploaded fonts |

**Use Yabe Webfont** as the primary recommendation.

---

## Font Subsetting

Strips unused characters. A full Latin font with all variants is often 200+ KB; subsetted to your charset can be 20–30 KB.

| Tool | Cost | Notes |
|------|------|-------|
| Font Squirrel | Free | https://www.fontsquirrel.com/tools/webfont-generator — granular optimizer |
| Everything Fonts | Free | https://everythingfonts.com/subsetter |
| Glyphhanger | Free | CLI tool |
| Subfont | Free | CLI tool |

Use Font Squirrel AND subset — they're complementary.

---

## Font Optimization Tools

| Tool | Notes |
|------|-------|
| FontForge (Free) | Edit font files; remove unnecessary glyphs; can reduce size 80–90% |
| Font Style Matcher (Free) | https://meowni.ca/font-style-matcher/ — match fallback to webfont (reduces layout shift) |
| WP FOFT Loader (Free) | Implements FOFT loading strategy to minimize FOIT/FOUT |

---

## Locally Host Third-Party Scripts

Every external domain call = additional latency from DNS lookup + connection.

### Automatic local hosting
| Plugin | Cost | Notes |
|--------|------|-------|
| GDPR Cache Scripts and Styles | Free | Locally caches external CSS/JS |
| Flyingpress | Paid | Recently added local hosting for all third-party files |

### Key files to always host locally
- Google Analytics / gtag.js → **CAOS (Free)**: https://wordpress.org/plugins/host-analyticsjs-local/
- Google Fonts → Yabe Webfont / OMGF
- Any plugin/theme fonts → download and upload manually

---

## Resource Hints

When you can't host locally, add resource hints to pre-establish connections.

### Pre-Party Browser Hints (Free)
https://wordpress.org/plugins/pre-party-browser-hints/
Supports: DNS prefetch, prerender, preconnect, prefetch, preload.

### Resource hint types (in order of power)
1. **preload** — downloads resource immediately with high priority
2. **preconnect** — opens TCP/SSL connection early to third-party domain
3. **dns-prefetch** — resolves DNS only; lighter than preconnect
4. **prefetch** — low-priority hint for future navigation resources
5. **prerender** — renders entire future page in background

### HTTP/2 Server Push vs Resource Hints
HTTP/2 Server Push > resource hints — server pushes resource before browser even requests it.
Use both together: Server Push for critical resources, hints for the rest.

**HTTP Push Content (Free)**: https://wordpress.org/plugins/http2-push-content/
Push JS/CSS files, apply async/defer, mobile/desktop specific rules.

---

## YouTube Performance

Standard YouTube embeds load 300+ KB on initial render.

**WP Youtube Lyte** — shows static placeholder; only loads YouTube player on click.
- Delay the WP Youtube Lyte JS file if video is below the fold
- If above the fold, don't delay (video won't appear until interaction)

---

## Analytics Third-Party Optimization

### Google Analytics
Delay these JS keywords/files in your JS delay plugin:
```
gtag.js
gtag.min.js
analytics.min.js
google-analytics.com
ga
/gtag/js
gtag(
/gtm.js
/gtm-
gtm.js
gtag
googletagmanager.com
```

### Google Tag Manager
1. Use **GTM Kit (Free)** for local hosting + delay
2. Or **GTM Server Side (Free)** — server-side GTM eliminates JS library entirely
3. Use one GTM container for all tracking tags (avoids multiple analytics script loads)

### Self-hosted analytics (best for privacy + performance)
| Plugin | Cost | Notes |
|--------|------|-------|
| Matomo | Freemium | Full-featured; more accurate than GA; GDPR-compliant |
| Koko Analytics | Free | Lightweight; basic stats |

### Heatmaps / Session recording
| Tool | Cost | Notes |
|------|------|-------|
| Microsoft Clarity | Free | Best free option; replaces Hotjar; session recording + heatmaps |
| Aurora Heatmap | Free | Local, no account required |

**Lazy Load Clarity (Free)**: https://wordpress.org/plugins/lazy-load-clarity/ — loads Clarity only after user interaction.

### A/B Testing (performance-conscious)
**Mojito (Free)**: https://github.com/mint-metrics/mojito — 5 KB JS; fastest A/B testing.
⚠️ Do NOT delay Mojito's JS — it must run immediately to determine which variant to show.

---

## HTTP Server Push

Smashing Magazine guide: https://www.smashingmagazine.com/2017/04/guide-http2-server-push/

**HTTP Push Content (Free)**: https://wordpress.org/plugins/http2-push-content/
- Push JS/CSS on load
- Async/defer any file
- Mobile/desktop conditional rules
- WooCommerce page-specific rules
