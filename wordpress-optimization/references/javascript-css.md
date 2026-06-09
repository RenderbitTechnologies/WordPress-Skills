# JavaScript & CSS Optimization — Reference

## Remove Unused CSS

### Instructions
1. Enable Remove Unused CSS in your optimization plugin
2. Clear all caches
3. Visit every page type on the site and check for design breakage
4. Add exclusions for broken CSS classes or full stylesheets
5. Clear unused CSS cache → clear all caches → retest

### Finding exclusions
- Inspect broken element → double-click CSS class name → copy
- Add to exclusions as `.classname` (with leading dot)
- If class exclusion fails, exclude the entire stylesheet (find it in GTMetrix/Debug Bear waterfall → filter CSS → copy filename)

### Pasteable Elementor exclusions (Perfmatters/Debloat)
Common Elementor selector exclusions: https://pastebin.com/j4QeL89d

### Plugins
| Plugin | Cost | Notes |
|--------|------|-------|
| Flyingpress | Paid | Best implementation |
| Perfmatters | Paid | Good exclusion control |
| Debloat | Free | Only free unused CSS option |
| Critical CSS for WP | Free | Removes unused + inlines critical |
| PurifyCss Online | Free | Manual approach: https://purifycss.online/ |

---

## Delay JavaScript — Full Plugin List

| Plugin | Cost | Notes |
|--------|------|-------|
| Perfmatters | Paid | Best; delay specific files OR delay-all with exclusions |
| Flyingpress | Paid | Integrated delay feature |
| Debloat | Free | Delay JS + remove unused CSS |
| WP-Meteor | Free | Delays ALL JS; configure exclusions |
| Flying Scripts | Free | Simple delay plugin |
| Optimize More! | Free | Multiple features including delay |

### Common JS files to delay (safe on most sites)
```
gtag.js
gtag.min.js
analytics.min.js
google-analytics.com
ga
/gtag/js
/gtm.js
gtm.js
klaviyo.min.js
wp-polyfill.min.js
regenerator-runtime.min.js
i18n.min.js
hooks.min.js
imagesloaded.js
jquery-migrate.min.js
flying-pages.min.js
scrolltrigger.min.js
gsap.min.js
wp-util.min.js
underscore.min.js
```

### Files to test before delaying (may break functionality)
- `hooks.min.js` — usually safe to delay, rarely needed immediately
- `wp-util.min.js` / `underscore.min.js` — safe to delay in most cases
- Any e-commerce checkout/cart JS — test carefully
- Menu-related JS — defer instead of delay if dropdown breaks

### Elementor delay list
Full list: https://pastebin.com/m94cSYku

---

## Defer Attribute

Use when delay breaks functionality but deferring is acceptable (e.g. menu dropdowns).

### Async Javascript (Free)
https://wordpress.org/plugins/async-javascript/
Applies async or defer attribute to JS files.

### Optimize More! (Free)
https://wordpress.org/plugins/optimize-more/
Can apply defer attribute.

---

## Minification

### HTML Minification
- **Minify HTML Markup (Free)**: https://wordpress.org/plugins/minify-html-markup/
- Most caching plugins also minify HTML

### CSS/JS Minification
- Use your caching plugin's built-in minification (Flyingpress, Speed Booster Pack, etc.)
- **Debloat (Free)**: minification + unused CSS removal
- Do NOT minify with multiple plugins simultaneously

### Manual tools
- CSS: https://cssminifier.com/
- JS: https://www.toptal.com/developers/javascript-minifier

---

## Inline Small CSS/JS Files

Only inline files **under ~3 KB** to reduce requests.

- **Optimize More! (Free)**: https://wordpress.org/plugins/optimize-more/ — inline JS + CSS
- **Asset Cleanup (Free/Pro)**: https://wordpress.org/plugins/wp-asset-clean-up/ — auto-inline CSS below threshold size

---

## Selectively Disable CSS/JS Assets

Remove specific files that aren't needed on certain pages.

| Plugin | Cost | Notes |
|--------|------|-------|
| Asset Cleanup Pro | Paid | Most configurable; disables WP Core files too |
| Perfmatters | Paid | Script Manager UI |
| WP Asset Manager (Gonzales) | Free | Full asset + plugin control |
| Optimize More! | Free | Selective disable |

---

## Selectively Disable Plugins Per Page

| Plugin | Cost |
|--------|------|
| Asset Cleanup Pro | Paid |
| Perfmatters | Paid |
| FreeSoul Plugin Deactivator | Free |
| Plugin Load Filter | Free |
| WP Plugin Manager | Free |
| Plugin Organizer | Free |

---

## Do NOT Concatenate/Combine Files

CSS/JS concatenation is a **legacy optimization** — do not use it.
It breaks JS delay, selective disabling, and other modern optimizations.
Disable any concatenation settings in your caching/optimization plugins.

---

## Remove Noscript Tags (Advanced)

Add to Fluent Snippets or functions.php:

```php
function remove_noscript_tags($html) {
    return preg_replace('/<noscript>(.*?)<\/noscript>/is', '', $html);
}
add_filter('the_content', 'remove_noscript_tags');
```
⚠️ Breaks functionality for users with JS disabled.

---

## Lazy Render HTML Elements

Defers rendering of below-fold HTML (like footers) until they enter viewport.

- **Delay/Lazy Load Anything (Free)**: https://wordpress.org/plugins/delay-load-any-content/
- **Perfmatters (Paid)**: built-in lazy render
- **Flyingpress (Paid)**: best; toggle per section in Elementor/Gutenberg editor
