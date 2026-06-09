# WordPress Caching — Reference

## The 8 Caching Layers (use all simultaneously)

```
User Browser
    ↓
DNS Cache (Cloudflare DNS)
    ↓
CDN Cache (Cloudflare, BunnyCDN)
    ↓
Server Cache (Varnish / NGINX FastCGI)
    ↓
Page/Application Cache (Flyingpress, Speed Booster Pack)
    ↓
PHP OPcache (precompiled PHP bytecode)
    ↓
Object Cache / DB Cache (Redis, Docket Cache)
    ↓
Browser Cache (static asset headers)
```

**Plus**: Service Worker Cache (advanced, significant benefit if implemented correctly)

---

## Page Caching Plugins

### Recommended
| Plugin | Cost | Notes |
|--------|------|-------|
| Flyingpress | Paid | Best; most features, active dev, responsive support |
| Speed Booster Pack | Free | Solid free option |
| Rapid Cache | Free | Fork of Comet Cache |
| Cache Enabler | Free | Pure HTML cache only |
| Hyper Cache | Free | Pure HTML cache only |
| Surge | Free | Minimal, pure cache |

### Avoid
**WP Rocket** — 700+ open GitHub issues, continuously removing features, stagnant development. Use Flyingpress instead.

### Only use ONE page caching plugin
Multiple page caching plugins cause conflicts and unsynchronized cache states.

---

## Cache Purging

**Rule**: When making changes, purge ALL of: page cache + object cache + CDN cache.

### The Cache Purger (Free)
https://wordpress.org/plugins/the-cache-purger/
Purges most common caching layers simultaneously. Does not purge service worker cache.

Supports: WP Rocket, Swift Performance, W3 Total Cache, Hummingbird, WP Fastest Cache, WP Super Cache, Kinsta, WPEngine, Cloudways, Varnish, Redis, Memcache, NGINX, PHP FPM, and more.

### Proxy Cache Purge (Free)
https://wordpress.org/plugins/varnish-http-purge/
Automatically purges Varnish/NGINX on content update.

---

## Object Caching (Database Cache)

Object caching reduces DB queries by storing results in memory.

| Plugin | Cost | Notes |
|--------|------|-------|
| Redis Object Cache | Free | Requires Redis extension in PHP; most performant |
| Docket Cache | Free | WP-level object cache; works without Redis; often faster than Redis for small sites |
| SQLite Object Cache | Free | Alternative to Redis |
| Memcache (Object Cache 4 Everyone) | Free | Memcached support |

### Redis optimization guide
https://www.dragonflydb.io/guides/redis-memory-and-performance-optimization

---

## Cache Warming

Pre-populate cache so first visitors get cached responses.

| Plugin | Cost |
|--------|------|
| Cache Warmer | Free |
| Warm Cache | Free |

---

## Fragment Caching (for highly dynamic sites)

When full-page caching isn't possible (WooCommerce cart, member areas), cache page fragments.

| Plugin | Cost | Notes |
|--------|------|-------|
| W3 Total Cache | Paid | Has fragment caching feature |
| Borlabs Cache | Free/Paid | Free version + pair with Cache Warmer |
| Content No Cache | Free | Exclude parts of page from cache |
| No Cache AJAX Widgets | Free | AJAX-powered widgets for non-cacheable content |

---

## AJAX Request Caching

| Method | Notes |
|--------|-------|
| Swift Performance Lite (Free) | Easiest; can cache AJAX GET requests |
| Cloudflare Super Page Cache | Can cache AJAX GET (not POST); uncheck "do not cache ajax requests" |
| Varnish | Guide: https://guides.wp-bullet.com/how-to-cache-ajax-get-requests-with-varnish-4/ |

**Limitation**: AJAX requests using WordPress nonces cannot be cached.

---

## PHP OPcache

Caches compiled PHP bytecode — PHP doesn't re-parse scripts on each request.

- Tuning guide: https://tideways.com/profiler/blog/fine-tune-your-opcache-configuration-to-avoid-caching-suprises
- **OPcache Manager (Free)**: https://wordpress.org/plugins/opcache-manager/ — management + analytics

---

## CDN Caching

Using a CDN reduces latency by serving from edge nodes geographically close to visitors.

| CDN | Notes |
|-----|-------|
| Cloudflare (Free/Pro) | Best free tier; also WAF, DNS, optimization features |
| BunnyCDN (Paid) | Performant; cost-effective |

### CDN is NOT a replacement for server-side caching
Even with 85–90% CDN cache hit ratio, 10–15% of requests still hit origin. Without proper origin-side caching, those users get slow responses.

### Cloudflare Super Page Cache (Free)
https://wordpress.org/plugins/wp-cloudflare-page-cache/
Integrates WordPress cache purging with Cloudflare cache.

### CDN URL rewrite
If self-managing CDN: use **CDN Enabler (Free)** to rewrite asset URLs to CDN domain.

---

## Browser Cache

Set via `.htaccess` or NGINX config. Most caching plugins handle this automatically.

Guide: https://onlinemediamasters.com/serve-static-assets-with-an-efficient-cache-policy-wordpress/

---

## Caching Layers — Request Flow

**First request** (nothing cached):
```
User → CDN (miss) → Server cache (miss) → WP cache plugin (miss) 
→ WP generates page → WP cache stores HTML → Server cache stores HTML 
→ CDN stores HTML → Browser gets page → Browser caches assets
```

**Subsequent requests** (all cached):
```
User → CDN (hit) → Serves cached HTML
```

**If CDN misses**:
```
User → CDN (miss) → Server cache (hit) → Serves from server cache
```

**If server cache misses**:
```
User → CDN (miss) → Server cache (miss) → WP cache (hit) → Serves WP cached HTML
```

---

## Specialized Caching

### Menu Cache
- **Docket Cache (Free)**: has menu caching in config panel
- **Menu Caching (Free)**: https://wordpress.org/plugins/menu-caching/
- **WP Nav Menu Cache (Free)**: https://wordpress.org/plugins/wp-nav-menu-cache/

### Translation Cache
- **Translation Cache (Free)**: https://wordpress.org/plugins/speed-up-translation/

### Gravatar Cache (if gravatars are used)
- **Optimum Gravatar Cache (Free)**: https://wordpress.org/plugins/optimum-gravatar-cache/

### REST API Cache
- **WP Rest Cache (Free)**: https://wordpress.org/plugins/wp-rest-cache/

### WP-Admin Cache
- **WP-Admin Cache (Free)**: https://wordpress.org/plugins/wp-admin-cache/
  After installing: Settings → WP Admin Cache → Check All → Save

---

## Remove Query Strings

Query strings reduce CDN cache hit ratio. Strip them where possible.

Guide: https://wpspeedmatters.com/ignore-query-strings/

---

## Service Worker Caching (Advanced)

Can significantly reduce payload size for repeat visitors.
- Philip Walton's guide: https://philipwalton.com/articles/smaller-html-payloads-with-service-workers/
