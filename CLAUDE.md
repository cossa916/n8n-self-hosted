# Cheshire Landscape Gardener (CLG) — working notes

This repo holds the self-hosted n8n setup plus website content work for
https://cheshire-landscape-gardener.com (WordPress + Divi builder, owner: Jason Costello).

## Content rules (owner's standing instructions)
- **Every new or updated page MUST include pictures or video clips.** Use existing
  media from the WordPress media library (it has well-tagged project photos and
  transformation videos under /wp-content/uploads/). Prefer recent project photos
  with descriptive alt text.
- Update existing ranking pages instead of creating new URLs (avoid keyword
  cannibalisation). Check GSC data first: /landscaping-costs-cheshire/ ranks ~7,
  /porcelain-patio-cost-cheshire/ ranks ~6.
- The site uses the **Divi builder** ([et_pb_*] shortcodes). Keep existing layout
  sections; append new sections as Divi shortcode modules. Never paste raw
  Gutenberg blocks into Divi pages.
- Back up a page's original JSON (context=edit) into `content/backups/` before
  changing it.

## Site facts
- Live domain: cheshire-landscape-gardener.com (hyphenated, non-www). The www
  variant 301-redirects to non-www — its GSC property showing "0 indexed" is normal.
- WordPress REST API is enabled; auth via Application Password (ask owner, do not
  store credentials in this repo).
- SEO titles/meta are managed by an SEO plugin (page title tag differs from WP title).

## SEO priority queue (from GSC, last 3 months)
1. ~~/landscapers-cheshire/~~ — DONE 2026-07-03 (video, services, costs, areas, FAQ + schema)
2. ~~/garden-design-cheshire/~~ — DONE 2026-07-03 (video, 3 images, featured image, FAQ schema, fixed 301 link, CTA)
3. ~~/landscape-gardener-wilmslow/~~ — DONE 2026-07-03 (Manchester Rd video, images, local content, FAQ + schema)
4. ~~/landscape-gardener-alderley-edge/~~ — DONE 2026-07-03 (reel video, images, local content, FAQ + schema)

Next candidates when owner wants more: /landscape-gardener-altrincham/ (1,774 imps,
pos ~23), /landscape-gardener-warrington/ (827 imps, pos ~42), /garden-redesign-in-knutsford/
(pos ~55). Owner should "Request indexing" in GSC for each updated URL.
