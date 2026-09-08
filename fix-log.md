# Open It on Your Phone — Fix Log

## Real Problems Found and Fixed

**1. About page photo was completely missing in production**
The photo `<img>` embed existed in a local build but was never actually uploaded to the
live `about.html` on GitHub — confirmed by checking the live page's rendered content, which
had no image tag at all. Fixed by re-downloading the correct file and re-uploading it to
overwrite the live version. Verified afterward on a real phone: photo now displays
correctly.

Before: About page with no photo, just heading and body text.
After: About page with the real photo displaying correctly above the bio text.
(Screenshots attached separately with this submission.)

**2. About photo was massively oversized for its display size**
The photo file was 810×820px (168KB), but only ever displayed at 160×160px on the page —
roughly 25x more image data than needed, a real mobile load-time cost. Resized to 320×320px
(2x display size, enough for retina screens) and recompressed. File size dropped from
168KB to 23KB (~86% smaller) with no visible quality loss at actual display size.

## Checked and Confirmed Working

- All nav links (Home, Case Study, About, Contact) resolve correctly on every page,
  verified via direct fetch, not just visual inspection.
- Text is readable on a real phone screen without zooming, on both headline and body sizes.
- The case study's "flagged/verified" tag styling and blockquote-style findings render
  cleanly on mobile — no overflow, no cramped spacing.
- Both case-study screenshots display fully, crisp, not blurry or cut off.
- Buttons ("Read the case study", nav links) are comfortably tappable, not cramped
  together.
- No layout breaks found on any of the four pages at phone width.

## Contrast (Verified Separately, Same Session)

Checked all identity-kit color pairs against WCAG AA using the actual hex values:
- Main text on background: 13.01:1 (passes everything)
- Secondary text on background: 6.33:1 (passes everything)
- Accent color (buttons/labels only, never body text): 4.35:1 (passes for large
  text/UI elements, which is the only place it's used on this site)
