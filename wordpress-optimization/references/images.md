# Images & Media Optimization — Reference

## Manual Compression Workflow

Always start with the highest quality version of the image. Repeat passes across services:

```
TinyPNG (3+ passes) → TinyJPG (2+ passes) → 11zon → TinyPNG → 11zon → TinyJPG → TinyPNG
```

Keep compressing until quality degrades unacceptably. **Target: under 100 KB per image.**

### Free compression services
| Service | URL |
|---------|-----|
| TinyPNG | https://tinypng.com/ |
| TinyJPG | https://tinyjpg.com/ |
| 11zon | https://imagecompressor.11zon.com/ |
| Compress-or-die | https://compress-or-die.com/ |
| Squeeze Pics | https://www.squeeze.pics/ |
| Batch Compress | https://batchcompress.com/en |
| Free Convert | https://www.freeconvert.com/ |

---

## Image Compression Plugins

| Plugin | Cost | Notes |
|--------|------|-------|
| ShortPixel | Freemium | Best compression per ThemeIsle benchmarks |
| Smush | Free/Pro | Generous free tier; unlimited under 5 MB |
| Converter for Media | Free | Auto WebP, fully local |
| Flying Images | Free | Multi-function; free CDN, on-the-fly WebP |
| WP Vivid Image Optimization | Freemium | 2000 images/month, fully featured |
| CompressX | Free | WebP + AVIF, local |
| Resmush.it | Free | API-based; JPG/PNG/GIF up to 5 MB |
| WP-Optimize | Free | Bulk compression |
| Bulk Image Optimizer | Free | Compress + resize + WebP, no limits |

---

## WebP and AVIF Conversion

| Plugin | Cost | Notes |
|--------|------|-------|
| Converter for Media | Free | Auto WebP |
| CompressX | Free | WebP + AVIF, local |
| WebP Express | Free | Serve auto-generated WebP |
| QuickWebP | Free | Full WebP, no fallback |
| AVIF Express | Free | AVIF with WebP fallback |
| PhastPress | Free | WebP + lazy load + CLS fixes |

---

## Lazy Loading

**Rule**: Lazy load ALL images below the fold. Exclude above-fold images explicitly.

| Plugin | Cost |
|--------|------|
| A3 Lazyload | Free |
| Rocket Lazyload | Free |
| Flying Images | Free |
| Auto-sizes for Lazy-loaded Images | Free |
| Perfmatters | Paid |
| Flyingpress | Paid |

### Identifying above-fold images to exclude
Check your lazy load plugin settings for an exclusion text box — paste the image filename.
Perfmatters auto-detects above-fold images but isn't perfect; verify in the waterfall.

---

## Fix CLS (Content Layout Shift) from Images

**Add width and height attributes** to every image — this is the primary CLS fix.

| Plugin | Cost | Notes |
|--------|------|-------|
| Optimize More! Images | Free | Adds missing width/height |
| Specify Missing Image Dimensions | Free | Single-function plugin |
| Perfmatters | Paid | Built-in |
| Flyingpress | Paid | Built-in |
| JCH Optimize | Free | Multi-function |
| PhastPress | Free | Multi-function |

---

## Image Resizing

| Plugin | Cost | Notes |
|--------|------|-------|
| Imsanity | Free | Auto-scale large uploads; bulk resize |
| Bulk Image Optimizer | Free | Compress + resize + WebP |
| Simple Image Sizes | Free | Custom image sizes |
| Better Image Sizes | Free | Dynamic generation, disable unused sizes |
| ImgProxy | Free | Self-hosted resize server |

---

## Adaptive/Responsive Images

| Plugin | Cost |
|--------|------|
| Adaptive Images | Free |
| ShortPixel Adaptive Images | Freemium |
| Enable Responsive Images for Gutenberg | Free |
| Flying Images | Free |

---

## Preload Images (LCP Optimization)

The LCP (Largest Contentful Paint) image **must be preloaded** for best scores.

| Plugin | Cost | Notes |
|--------|------|-------|
| Image Prioritizer | Free | Auto-detects LCP by breakpoint |
| Fetch Priority Plugin | Free | Official WP performance team plugin; auto-applies |
| Preload Featured Images | Free | Auto-preloads featured images |
| Perfmatters | Paid | Explicit fetch priority + preload control |
| Pre-Party | Free | Adds preload attribute |
| Lazyload, Preload and More | Free | Preload + eager loading attribute |

---

## Icon Optimization

Replace Font Awesome with individual SVGs. Font Awesome loads 90+ KB for possibly 1–2 icons.

### Free SVG icon sources
- FontAwesome: https://fontawesome.com/download
- FlatIcon: https://www.flaticon.com/
- IconFinder: https://www.iconfinder.com/

### SVG compression (use both together)
- Vecta.io: https://vecta.io
- SVGOMG: https://jakearchibald.github.io/svgomg/

### Inline SVGs into Elementor (custom plugin)
https://github.com/jazir555/Inline-SVGs-uploaded-to-Elementor-Icon-Widget

---

## Video Optimization

### Lazy load video iframes
- **Lazy Embed (Free)**: https://wordpress.org/plugins/lazy-embed/
- **Embed Optimizer (Free)**: https://wordpress.org/plugins/embed-optimizer/ — for YouTube, TikTok etc.
- Perfmatters and Flyingpress also lazy load video iframes

### Video compression
- **Handbrake (Free)**: https://handbrake.fr/ — convert to WebM (~30% smaller than MP4)
- **Free Convert**: https://www.freeconvert.com/video-compressor

### YouTube performance
Never use standard YouTube embeds — they load 300+ KB immediately.
Use **WP Youtube Lyte** — shows a placeholder image until the user clicks play.
Delay the WP Youtube Lyte JS file if the video is below the fold.

### Lightweight video players
- Presto Player (Free): HTML5, YouTube, Vimeo
- Super Video Player (Free): Self-hosted MP4/OGG
- Video Pack (Free): Local hosting

---

## GIF Optimization

| Service | URL |
|---------|-----|
| Free Convert | https://www.freeconvert.com/gif-compressor |
| Ezgif | https://ezgif.com/optimize |
| Gifcompressor | https://gifcompressor.com/ |
| ILoveIMG | https://www.iloveimg.com/compress-image/compress-gif |

---

## Lottie Animation Optimization

- **AM Lottie Player (Free)**: https://wordpress.org/plugins/am-lottieplayer/
- **Lottiemizer (Free)**: https://www.lottiemizer.com/
- **dotLottie**: https://dotlottie.io/ — compressed `.lottie` format

---

## Reduce Image Colors

Reducing color palette reduces file size without always affecting visual quality.

- Decrease PNG colors: https://onlinepngtools.com/decrease-png-color-count
- Reduce WebP colors: https://onlinetools.com/webp/reduce-webp-colors
- Reduce JPG colors: https://onlinejpgtools.com/reduce-jpg-colors

---

## Dynamic Image Caching (Advanced)

For real-time photo galleries: use IndexedDB to cache images client-side. Steps:
1. Open IndexedDB database
2. On fetch, store image blob with URL as key
3. Check cache before fetching
4. Implement update strategy for freshness
