# Themes & Page Builders — Reference

## Theme Recommendations for Page Builder Users

| Theme | Cost | Notes |
|-------|------|-------|
| Hello Elementor | Free | Minimal footprint; 3× smaller than Picostrap; best for Elementor |
| Picostrap | Free | Very lightweight; created by Live Canvas team |

**Avoid Astra for new builds** — adds 300+ KB of JS/CSS. Delaying its JS breaks functionality.

---

## Optimizing Astra (if already using it)

Download the Astra child theme: https://wpastra.com/child-theme-generator/

Add to child theme's `functions.php` under "define constants":

```php
function astra_remove_header() {
    remove_action( 'astra_masthead', 'astra_masthead_primary_template' );
}
add_action( 'wp', 'astra_remove_header' );

add_filter( 'astra_schema_enabled', '__return_false' );
add_filter( 'astra_meta_box_options', 'default_disable_options' );

function default_disable_options( $meta_option ) {
    $meta_option['ast-main-header-display'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['footer-sml-layout'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['footer-adv-display'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['site-post-title'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['site-sidebar-layout'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['site-content-layout'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['ast-featured-img'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    $meta_option['ast-breadcrumbs-content'] = array('default' => 'disabled', 'sanitize' => 'FILTER_DEFAULT');
    return $meta_option;
}

add_filter( 'astra_amp_support', '__return_false' );
```

Do NOT use Astra Pro — it makes performance worse.

---

## Page Builder Performance Ranking

### Fast (pre-optimized clean code)
1. **Oxygen Builder (Paid)** — https://oxygenbuilder.com/ — clean HTML, minimal DOM
2. **Bricks Builder (Paid)** — https://bricksbuilder.io/ — clean code output
3. **Live Canvas (Paid)** — https://livecanvas.com/ — Bootstrap 5 HTML output; pair with Picostrap

### Good after optimization
4. **Elementor (Freemium)** — very bloated pre-optimization; one of the lightest post-optimization

### Avoid
- **WPBakery** — avoid entirely
- **Beaver Builder** — massive CSS bloat (135 KB per section for shape dividers); cannot be removed with unused CSS tools
- **Divi** — unremovable cruft; some improvements possible but not fully fixable

---

## Elementor Optimization Stack

### Target: 90+ mobile PageSpeed score on any Elementor site

#### Plugin stack (paid)
1. Advanced Database Cleaner Pro
2. Index WP MySQL For Speed (Free)
3. Asset Cleanup Pro
4. Perfmatters (Paid)
5. Disable Gutenberg (Free)
6. Docket Cache (Free)
7. Pre-Party (Free)
8. Flyingpress (Paid)
9. Flying Pages (Free) — delay flying-pages.min.js
10. Inspect HTTP Requests (Free)
11. Disable WordPress and WooCommerce Bloat (Free)
12. Hello Elementor theme (Free)

#### Free equivalent of Perfmatters
1. Asset Cleanup (Free)
2. Debloat (Free)
3. Pre-Party (Free)
4. Remove Global Styles (Free)
5. Disable WordPress and WooCommerce Bloat (Free)
6. CAOS for Local Analytics (Free)
7. Freesoul Plugin Deactivator (Free)
8. Fluent Snippets (Free)
9. A3 Lazyload (Free)
10. Dynamic Heartbeat Control (Free)
11. CDN Enabler (Free)
12. Yabe Webfont (Free)
13. Disable Google Fonts (Free)
14. WPS Hide Login (Free)
15. Lazy Load Anything (Free)
16. Speed Booster Pack (Free)

#### Pasteable configuration lists
- JS delay list: https://pastebin.com/m94cSYku
- CSS exclusions: https://pastebin.com/j4QeL89d

---

## Elementor Settings to Enable

### Performance Experiments (Settings → Features)
Many are disabled by default — enable all that apply:
- Optimized Control Loading (major improvement for large sites)
- Lazy Load for Images
- All other experiment toggles

### CSS Print Method
Change to **Internal Embedding** → eliminates `post-xxxx.css` file requests by inlining them.
Test first: if too much CSS is inlined it can hurt performance.

### Best practices
- Use Hello Elementor theme only
- Minimize containers/sections/columns (each = additional DOM element)
- Never set background images on non-standard elements (can't be lazy loaded)
- Use individual SVG icons, not Font Awesome library
- Limit use of animations (each loads additional JS)
- Use grid containers instead of columns where possible

---

## Elementor Free → Pro Features (Free Alternatives)

| Pro Feature | Free Alternative |
|-------------|-----------------|
| All Pro features | **Pro Elements (Free)**: https://proelements.org/ |
| Custom CSS | RRdevs, Ultra Addons Lite, Xpro Addons |
| Theme Builder | Xpro Theme Builder |
| Dynamic Tags | Royal Addons |
| Dynamic Visibility | Dynamic Visibility for Elementor |
| Sticky Header | JetSticky For Elementor |
| Mini Cart | Ultra Addons Lite |
| Animations | UiCore Animate |
| Header & Footer | Header & Footer Builder plugin |

---

## Elementor Addon Packs

| Pack | Cost | Notes |
|------|------|-------|
| Xpro Addons | Free | Custom CSS + premium widgets |
| Superb Addons | Free | Editor enhancements + templates |
| LA-Studio Element Kit | Free | Widget kit + templates |
| Hash Elements | Free | Free addon elements |
| Image Hover Effects | Free | Lightweight hover effects |

---

## Elementor Template Kits

| Kit | Cost |
|-----|------|
| SKT Templates | Free |
| Rife Extensions | Free |
| Templately | Free/Pro |
| Envato Elements | Free/Pro |

---

## Import Figma to Elementor

**UIChemy (Free/Pro)**: https://wordpress.org/plugins/uichemy/ — 10 imports/month free; workaround via InstaWP for unlimited.
