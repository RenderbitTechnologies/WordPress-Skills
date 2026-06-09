# Database Optimization — Reference

## ⚠️ Always Back Up Before Any Database Changes

Autoload changes in particular can cause critical errors or lock you out of WP-Admin.
Test ALL database changes on staging first.

---

## Autoloaded Options (Highest Impact)

Autoloaded data loads on every single page request. Disabling unnecessary autoloads reduces DB overhead significantly.

### Diagnosis plugins
| Plugin | Cost | Notes |
|--------|------|-------|
| AAA Option Optimizer | Free | Tracks which options are actually used during browsing |
| SweepPress | Free | Shows all autoloaded options; 43 cleanup categories |
| Autoload Checker | Free | Shows top autoloaded entries by size |
| Advanced Database Cleaner | Freemium | Full-featured database manager |
| Database Cleaner | Free | Another option for autoload management |

### Process
1. Install AAA Option Optimizer
2. Browse every page type on your site
3. After a week (or full site visit), check what's flagged as unused
4. Set flagged options to `autoload=no` via SweepPress or ADC
5. Test every page to confirm nothing broke
6. Revert any that break functionality

---

## Database Indexing

Indexes dramatically speed up query lookups on large databases.

**Do not use Index MySQL for Speed and Scalability Pro simultaneously.**

| Plugin | Cost | Notes |
|--------|------|-------|
| Index WP MySQL For Speed | Free | Adds missing indexes to WP core tables |
| Index WP Users For Speed | Free | Indexes user tables; deactivate after adding indexes |
| Scalability Pro | Paid | Best for WooCommerce; indexes WC tables too |
| Meta Optimizer | Free | Optimizes meta data storage; converts myISAM → InnoDB |
| Templ Optimizer | Free | Auto-converts myISAM to InnoDB |

> **Note**: Deactivate Index WP Users For Speed after adding indexes — it adds excessive DB calls when active.

---

## Database Cleaners

Orphaned data from deinstalled plugins, excess revisions, and expired transients all slow queries.

| Plugin | Cost | Notes |
|--------|------|-------|
| SweepPress | Free | 43 cleanup categories; best free option |
| WP-Sweep | Free | Classic, reliable sweeper |
| Advanced Database Cleaner | Freemium | Most full-featured |

### Always run after deleting a plugin
Most plugins leave tables and options behind. Use ADC or SweepPress to remove them.

---

## Query Analysis

Identify slow queries and which plugins/themes cause them.

| Plugin | Cost | Notes |
|--------|------|-------|
| Query Monitor | Free | Best diagnostic tool; shows slow queries, duplicate queries, total query time |
| Fluent Query Logger | Free | Selective query logging |

### Query Monitor workflow
1. Open WP admin while logged in
2. Click "Query Monitor" in admin bar
3. Check "Queries by Caller" tab — see total time per plugin
4. Check "Duplicate Queries" tab — repeated queries indicate inefficiency
5. Total query time should be < 0.03 seconds for a well-optimized site

### Query Monitor add-ons
https://querymonitor.com/help/add-on-plugins/

---

## MySQL Configuration Tuning

### Automatic tuning
| Tool | Cost | Notes |
|------|------|-------|
| MySQLTuner | Free | Perl script; analyzes DB and suggests config changes |
| Releem | Paid | Auto-tunes MySQL config based on current and average load |

### Manual tuning
Run MySQLTuner, apply its recommendations to `/etc/mysql/my.cnf`.

---

## Transients

Expired transients clutter the wp_options table and slow autoload.

| Plugin | Cost |
|--------|------|
| Transients Manager | Free |
| Delete Expired Transients | Free |

---

## High CPU Load / Excessive RAM Pressure

### Common causes
1. Malware infection
2. DDoS / brute force attack
3. Excessive autoloaded options
4. Too many WP-Admin tabs open (Heartbeat API)
5. Poorly coded plugins/themes
6. Missing/misconfigured caching
7. Excessive WordPress cronjobs

### Diagnosis
| Plugin | Cost | Notes |
|--------|------|-------|
| F12 Profiler | Free | Shows per-plugin load time in admin bar |
| Code Profiler | Free/Pro | PHP-level profiling; identifies exact lines causing load |
| New Relic (OSS) | Free | Server-level observability and performance monitoring |
| SigNoz | Free | Alternative to New Relic; Docker-based |

### Heartbeat API control
WP Heartbeat makes AJAX calls every 15–60 seconds — multiplied by open admin tabs = significant load.

| Plugin | Cost |
|--------|------|
| Dynamic Frontend Heartbeat Control | Free |
| Admin and Site Enhancements | Free |

### WordPress Cronjobs
Disable WP Cron (it runs on page load) and replace with actual server cron jobs.

Guide: https://www.wpbeginner.com/wp-tutorials/how-to-disable-wp-cron-in-wordpress-and-set-up-proper-cron-jobs/

Cron generators:
- https://crontab.cronhub.io/
- https://crontab.guru/

**WP Crontrol (Free)**: https://wordpress.org/plugins/wp-crontrol/ — view and manage WP cron jobs
**Cron Logger (Free)**: https://wordpress.org/plugins/cron-logger/ — identify problematic cron jobs

---

## Object Caching (see caching.md for full details)

- **Redis Object Cache (Free)** — requires Redis PHP extension
- **Docket Cache (Free)** — WP-level object caching, no Redis needed

---

## WP Database Administration Tools

| Plugin | Cost | Notes |
|--------|------|-------|
| Database Admin | Free | Adminer replacement in WP admin |
| WP phpMyAdmin | Free | phpMyAdmin inside WP admin |
| WP Database Snapshots | Free | Fast rollback snapshots during testing |
| Enable Database Tools | Free | Optimize/repair InnoDB and MyISAM tables |
