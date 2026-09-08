# Break Your Own Site — Where It Breaks

## What I Actually Tried

- Submitted the contact form empty
- Submitted the contact form with garbage text
- Clicked Send twice, fast
- Clicked every nav link and both case-study action links
- Searched my own name/site on Google
- Ran PageSpeed Insights (mobile)
- Found a caching issue that gave misleading first-run results, and re-verified once I 
  understood why

## Findings, Sorted

### Fix-Now (Fixed)

**1. Double-submit was possible.** The Send button didn't disable after clicking, so a fast 
double-click could submit the form twice. Fixed: button now disables and shows "Sending…" 
on click, re-enables only if something goes wrong.

**2. No social-share preview on any page.** Pasting a link into LinkedIn/WhatsApp/Slack 
showed a bare link with no title/description card. Fixed: added Open Graph and Twitter 
Card meta tags to all four pages.

**3. Images had no width/height attributes,** causing layout shift as they loaded (a real 
Core Web Vitals issue, confirmed by PageSpeed). Fixed: added explicit dimensions to all 
three images.

**4. Missing `<main>` landmark**, flagged by the accessibility audit — assistive technology 
had no way to identify the primary content region on any page. Fixed: wrapped each page's 
content in a `<main>` element.

**5. Accent color failed contrast for small text.** PageSpeed's automated contrast checker 
flagged the flag-red accent (#C1442E, 4.35:1 against the paper background) as insufficient. 
Fixed: darkened the accent to #A83A27 (5.45:1, passes AA) across the whole site — a small, 
deliberate identity-kit update, not just a one-off patch.

**6. A real deployment bug found mid-audit, unrelated to the above:** a copy-paste upload 
method (Notepad → GitHub's edit box) had silently dropped the `<!DOCTYPE html>` line on 
live pages, which PageSpeed's first run correctly caught (quirks-mode, missing title/lang 
detection cascading from the malformed start of the file) even though the source file 
itself was correct. Fixed by switching to direct file upload (drag-and-drop) instead of 
copy-paste for all HTML files going forward — removes the whole class of bug.

### Known Limitations (Not Fixed, Named Honestly)

**1. Not yet findable via Google search.** Searching "Lanre Odesanya ML portfolio" returns 
unrelated people, not this site. This is expected for a brand-new site with no backlinks 
and no search-engine submission yet — real SEO indexing takes time this task's window 
doesn't cover. Next step would be submitting the sitemap to Google Search Console.

**2. Google Fonts load render-blocking**, costing an estimated 1.8 seconds on slow 
connections per PageSpeed. Fixing this properly means self-hosting the fonts or using 
font-display strategies — a real improvement, deliberately deferred rather than rushed 
given the plain-HTML, no-build-step constraint this whole site was built around.

**3. Garbage text in the contact form is accepted.** The form blocks empty submissions 
(HTML5 `required`) but doesn't validate that the message field contains anything 
meaningful — someone could type "asdf" and it would send. Accepted as a real, minor 
limitation; Formspree's own spam filtering is the actual backstop here, not client-side 
validation.

## Hardening Review

Submitted this list, the live URL, and the fixes above for review — must-fixes addressed 
before this checkpoint is considered closed.
