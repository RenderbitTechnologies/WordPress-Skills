---
name: wordpress-optimization
description: >
  Expert WordPress performance optimization guide covering speed testing, caching, image optimization, JS/CSS
  optimization, database tuning, security, themes, page builders, and Core Web Vitals. Use this skill whenever
  the user mentions WordPress performance, pagespeed, slow WordPress site, Core Web Vitals, TTFB, LCP, CLS,
  FCP, caching plugins, image optimization, JS delay, removing unused CSS, Elementor performance, WP Rocket,
  Flyingpress, Perfmatters, WP hosting, or any WordPress speed/optimization topic — even if they only ask a
  single quick question. Also trigger for questions about WordPress plugins related to performance, security
  hardening that affects speed, or database optimization.
---

# WordPress Performance Optimization

## Core Philosophy

1. **Mobile scores only matter** — Google ranks by mobile PageSpeed. Desktop is irrelevant. Always test and optimize for mobile.
2. **Target 90+ on Google PageSpeed Insights (mobile)** — achievable on virtually any site with this guide.
3. **Lab data is your optimization target** — synthetic (lab) tests are what you optimize; field data lags 30 days.
4. **Debug Bear for diagnosis, PageSpeed Insights for the final score** — use Debug Bear's waterfall for what to fix, PSI to verify progress.
5. **Average 3 tests** — there is always inter-test variance; never judge a single run.

---

## Speed Testing Tools

| Tool | Purpose |
|------|---------|
| [PageSpeed Insights](https://pagespeed.web.dev/) | The only mobile score that matters for SEO |
| [Debug Bear](https://www.debugbear.com/test/website-speed) | Best diagnostic tool; waterfall chart, JS/CSS lists |
| [GTMetrix](https://gtmetrix.com) | Good waterfall; shows page weight and request count |
| [ByteCheck](https://www.bytecheck.com/) | TTFB breakdown by component (DNS/Connect/SSL/Send/Wait/Receive) |
| [Yellow Lab Tools](https://yellowlab.tools/) | Unique additional metrics |
| [Experte](https://www.experte.com/pagespeed) | Bulk test multiple pages at once |
| [Lightest App](https://lightest.app/) | Competitor comparison |

**Good TTFB targets:** Desktop < 300 ms · Mobile < 500 ms. Over these = optimization opportunities remain.

**Good page weight:** ≤ 500 KB. Ideal ≤ 168 KB (achievable with JS delay + lazy load).

**Good request count:** Under 40 requests is the sweet spot.

---

## Optimization Priority Order (by impact)

1. **Delay all possible JavaScript** — single largest page speed gain
2. **Remove unused CSS** — eliminates dead CSS weight
3. **Image compression + lazy load** — often halves page weight
4. **Caching (all 8 layers)** — page cache, object cache, CDN, server cache
5. **TTFB optimization** — hosting quality, server-side caching, database
6. **Locally host fonts + third-party files** — eliminates external latency
7. **Database optimization** — autoload options, indexes, query cleanup
8. **Redirect chain elimination** — remove intermediary HTTP→HTTPS→WWW hops
9. **Preload inner pages** — Flying Pages / Speculation Rules
10. **Minify HTML/CSS/JS** — smaller payload

---

## JavaScript Optimization

### Delay (best) vs Defer vs Async

- **Delay**: JS does not download until user interaction → zero performance impact. Use whenever functionality isn't needed on initial render.
- **Defer**: JS downloads in background, executes after DOM parsed. Use when delay breaks functionality (e.g. menu dropdowns).
- **Async**: Only for analytics where strict accuracy is needed. Generally avoid.

> **Delay is 3–5× more impactful than defer.** Always try delay first; fall back to defer if functionality breaks.

### Identifying what to delay
1. Run Debug Bear/GTMetrix → filter waterfall to JS files
2. Delay each file individually; test site after each change
3. Add exclusions for files that break functionality

### Common safe-to-delay files
`analytics.js`, `gtag.js`, `klaviyo.min.js`, `wp-polyfill.min.js`, `regenerator-runtime.js`,
`i18n.min.js`, `hooks.min.js`, `imagesloaded.js`, `jquery-migrate.min.js`, `flying-pages.min.js`,
all slider JS, all animation JS below the fold, cookie banners, social pixels, tag manager scripts.

**Do NOT delay**: lazysizes.js, any JS that must run on above-fold interactive elements, Mojito A/B test JS.

### Plugins
- **Perfmatters (Paid)** — most granular control; delay specific files + Delay All JS
- **Debloat (Free)** — delay JS + remove unused CSS
- **WP-Meteor (Free)** — delays all JS with exclusions
- **Flying Scripts (Free)** — simple delay plugin

See `references/javascript-css.md` for full plugin list and Remove Unused CSS instructions.

---

## CSS Optimization

1. **Remove Unused CSS** — largest CSS win. Inline critical CSS into HTML; strip the rest.
2. **Do NOT combine/concatenate CSS or JS files** — legacy technique, breaks modern optimizations.
3. **Inline small CSS files** (< 3 KB) to reduce requests.

### Remove Unused CSS plugins
- **Flyingpress (Paid)** — best implementation
- **Perfmatters (Paid)** — solid with exclusion control
- **Debloat (Free)** — only free option for unused CSS removal
- **Critical CSS for WP (Free)** — removes unused + inlines critical

**Adding exclusions**: If design breaks → Inspect element → copy CSS class → add `.classname` to exclusions box → clear cache → retest.

---

## Image Optimization

**Goal**: All images under 100 KB. A 400 KB difference can swing scores by 10–15 points.

### Manual compression workflow
TinyPNG → TinyPNG (repeat 3×) → TinyJPG → TinyPNG → 11zon → TinyPNG → repeat until no further reduction.

Bounce between services; each uses slightly different algorithms allowing further passes.

### Key rules
- Compress before uploading; multiple passes always
- Convert to **WebP** (or AVIF for cutting-edge) with fallback
- **Lazy load all below-fold images** — exclude above-fold images from lazy load
- Add `width` and `height` attributes to all images (prevents CLS)
- Use WOFF2 for fonts, not TTF
- Replace Font Awesome icon libraries with individual SVG files

### Recommended plugins
- **ShortPixel (Freemium)** — best overall compression per ThemeIsle benchmarks
- **Smush (Free/Pro)** — generous free tier, unlimited images < 5 MB
- **Converter for Media (Free)** — auto WebP conversion, fully local
- **Flyingpress (Paid)** — compression + lazy load + CDN
- **Flying Images (Free)** — multi-function, free CDN, on-the-fly WebP

See `references/images.md` for full tool list, resize, adaptive images, and CLS fix plugins.

---

## Caching (All 8 Layers)

**Use all layers simultaneously — they are complementary, not redundant.**

| Layer | Tools |
|-------|-------|
| Page/Application cache | Flyingpress, Speed Booster Pack, Cache Enabler |
| Server cache | Varnish, NGINX FastCGI |
| Object cache (DB) | Redis, Docket Cache (WP-level alternative to Redis) |
| PHP OPcache | Built into PHP; tune via php.ini |
| CDN cache | Cloudflare, BunnyCDN |
| Browser cache | Set via headers/htaccess |
| Service worker cache | Advanced; see Smashing Magazine article |
| DNS cache | Use Cloudflare as DNS provider |

### Page caching plugin recommendation
**Flyingpress (Paid)** — best featureset, active development, responsive developer.

Free alternative stack: Speed Booster Pack + Cache Enabler + Hyper Cache

**Avoid WP Rocket** — 700+ open bugs, continuously removing features, stagnant development.

### Critical rule: Purge all 3 server-side layers together
Page cache + Object cache + CDN cache must all be cleared together after any change.
Use **The Cache Purger (Free)** plugin for bulk purging.

See `references/caching.md` for full caching layer details, fragment caching, AJAX caching.

---

## Database Optimization

1. **Disable unnecessary autoloaded options** — set to `autoload=no` via SweepPress or AAA Option Optimizer. Test on staging first — wrong settings can lock you out.
2. **Add indexes** — use Index WP MySQL For Speed (Free) or Scalability Pro (Paid, especially for WooCommerce).
3. **Clean database** — remove orphaned tables/options after uninstalling plugins (Advanced Database Cleaner).
4. **MySQL tuning** — run MySQLTuner for automatic recommendations.
5. **Object caching** — Redis or Docket Cache to reduce DB queries.
6. **Delete expired transients** — use Delete Expired Transients plugin.

> ⚠️ Always take a database backup before changing autoload settings. Critical errors can occur.

---

## Themes & Page Builders

### Theme recommendation for page builder users
**Hello Elementor (Free)** — minimal footprint, best starting point.
**Picostrap (Free)** — also excellent, created by Live Canvas team.

**Avoid Astra** — adds 300+ KB of unnecessary JS/CSS. If stuck on Astra, see `references/themes-builders.md` for optimization code.

### Builder performance ranking (post-optimization)
1. **Oxygen / Bricks / Live Canvas** — clean output, pre-optimized
2. **Elementor** — bloated pre-optimization, but one of the lightest *after* optimization
3. **Avoid**: WPBakery (terrible), Divi (unremovable cruft), Beaver Builder (massive CSS bloat)

### Elementor-specific optimizations
- Enable **Performance Experiments** in Settings → Features (many disabled by default)
- Use **CSS Print Method: Internal Embedding** to inline post-xxx.css files
- Use **Hello Elementor** theme only
- Limit containers/sections/columns (each adds DOM elements)
- Never set background images on non-standard elements (can't be lazy loaded)
- Disable all unused Elementor widgets via Element Manager

---

## TTFB Optimization

TTFB has 6 components: **DNS → Connect → SSL → Send → Wait → Receive**

| Component | Fix |
|-----------|-----|
| DNS | Use Cloudflare as DNS provider |
| Connect | TCP optimization on VPS |
| SSL | TLS 1.3, session resumption, fast cipher suites |
| Send | Brotli/GZIP compression, HTTP/2 or HTTP/3 |
| Wait (server processing) | WordPress optimization — this is the big one |
| Receive | Reduce initial payload size, HTTP/2 multiplexing |

**Wait time is reduced by**: page caching, object caching, database optimization, disabling heavy plugins, and all WordPress-level optimizations in this guide.

---

## Fonts

- Use **WOFF2** format only
- **Locally host all fonts** — never load from Google's CDN
- **Subset fonts** using Font Squirrel or Glyphhanger to strip unused characters
- **Preload** above-fold font files
- Replace icon font libraries (Font Awesome) with individual SVGs

**Best font hosting plugin**: **Yabe Webfont (Free)** — most flexible, GDPR-friendly.
Alternative: OMGF (Free), or Flyingpress (Paid, can't serve manually uploaded fonts).

---

## Third-Party Files

**Rule**: Host everything locally. Every external domain = additional latency.

- Google Fonts → locally host via Yabe Webfont
- Google Analytics → locally host via CAOS
- All analytics scripts → delay to eliminate performance impact
- YouTube embeds → use **WP Youtube Lyte** (placeholder image until user clicks)
- Captchas → avoid; use Cloudflare's captcha or form-level alternatives instead

---

## Security (Performance-Conscious)

- **Do NOT use Wordfence on live sites** — costs 10–15 pagespeed points. Use only on staging for scans.
- **3-layer firewall**: WAF (Cloudflare) + Server level (7G/8G firewall) + App level (NinjaFirewall or BBQ)
- **Disable XML-RPC** — prevents DDoS/brute-force amplification (use Asset Cleanup or Simple Disable XML-RPC)
- **Patchstack (Freemium)** — virtual patches before plugin updates
- **Two Factor (Free)** — official WP 2FA plugin from core team
- Scan on staging only: use GOTMLS for lightweight malware scanning

---

## WordPress Best Practices

- **Always test plugins on staging first** — some leave permanent DB artifacts even after removal
- **Selective plugin disabling** — disable plugins on pages where they aren't needed (Asset Cleanup Pro / Perfmatters / FreeSoul Plugin Deactivator)
- **Disable XML-RPC** unless a plugin explicitly requires it
- **Set WP_MEMORY_LIMIT to 256M** in wp-config.php (max 512M)
- **Disable WP Cron** and replace with server cron jobs
- **Control WordPress Heartbeat** — reduce interval to lower admin-ajax load
- **Keep backups** before every change, especially database changes
- **Debug and analysis plugins** — activate on staging only; they cause performance hits

---

## Common Misconceptions (Correct These)

| Misconception | Truth |
|---------------|-------|
| Desktop scores matter | Only mobile scores affect Google ranking |
| Page builders can't be fast | Elementor can hit 90+ once optimized |
| Server-level cache replaces WP caching plugins | They're complementary layers |
| Too many plugins = slow site | It's the quality of code that matters, not count |
| Pagespeed lab scores don't matter | They're your primary optimization target |
| Bloated DOM always hurts | DOM size only hurts if HTML document weight is also high |
| Security plugins are required on live | Wordfence kills performance; use layered lightweight alternatives |

---

## Reference Files

For detailed plugin lists, specific configurations, and extended guidance:

- `references/javascript-css.md` — JS delay/defer details, Remove Unused CSS exclusion guide
- `references/images.md` — Full image tool list, video, SVG, lazy load, CLS fixes
- `references/caching.md` — All 8 caching layers, fragment caching, AJAX caching
- `references/database.md` — Autoload, indexes, query analysis, object caching
- `references/themes-builders.md` — Elementor plugin stack, Astra optimization code, builder comparisons
- `references/security.md` — Firewall setup, security plugin recommendations
- `references/fonts-third-party.md` — Font subsetting, resource hints, local hosting
- `references/analytics.md` — GA/GTM optimization, self-hosted analytics, heatmaps

Read the relevant reference file when the user needs specific plugin recommendations or detailed configuration steps for that topic area.
