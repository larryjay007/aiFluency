# Three Roads — Choosing My Stack

## My Constraints

- **Free only:** no paid tools, no paid hosting.
- **Honest skill level:** beginner in programming — need step-by-step guidance for Git/GitHub
  operations, not yet comfortable debugging build tools or dependency errors independently.
- **What the portfolio needs to do:** walk a startup founder from claim → proof (real
  screenshots + case study text) → belief → one action (email). Four pages: Home, Case
  Study, About, Contact.
- **How work must be displayed:** real screenshots (notebook outputs), case study prose. No
  interactive demo yet (the Lane 2 tool doesn't exist yet), no image gallery, no long-form
  blog.
- **Does anything need to be dynamic yet?** No — everything is static content for now.

---

## Three Options Considered

| | How you'd build | Free host | Backend needed? | Real trade-off |
|---|---|---|---|---|
| **1. Plain HTML/CSS** | Hand-write each page directly | GitHub Pages | No | Zero tooling to learn, but repeating the nav/footer on every page means editing 4 files by hand for any shared change |
| **2. Static site generator (Jekyll/Eleventy)** | Write content once in templates, generator builds the pages | GitHub Pages (Jekyll native) or Netlify | No | Solves the repetition problem, but adds a build step and new failure modes — a broken build can take the whole site down |
| **3. Framework (Next.js) or no-code (Webflow)** | Component-based code or visual builder | Netlify/Vercel free tier | Not required now, but tooling assumes I'll eventually want one | Most powerful and future-proof for an eventual live demo, but steepest learning curve and a deploy pipeline I couldn't easily debug alone |

---

## Pressure-Testing the Front-Runner (Plain HTML/CSS)

- **What breaks if I pick the simplest?** Nothing today — the real cost shows up later: if
  the site grows to many case studies, updating shared elements (nav, footer) means editing
  every file by hand.
- **What would I maintain if I picked the most powerful?** A build pipeline, dependency
  versions, framework updates, and deploy failures — real ongoing maintenance I don't
  currently have the skill to troubleshoot independently.
- **Can I finish in two weeks?** Already did — built and live in well under that.
- **Does it show my work the way it needs to be shown?** Yes — real screenshots and case
  study prose are exactly what plain HTML displays well. Nothing I actually need (no gallery,
  no live demo yet) requires anything Option 1 can't do.

---

## Decision & Rationale

I chose plain HTML/CSS on GitHub Pages. I considered a static site generator like Jekyll,
since it would save me from repeating the same navigation on every page — but that adds a
build step and a new way for things to break, and I'm not yet confident I could debug a
broken build on my own. I also considered a React/no-code stack like Next.js on Vercel,
which would make the most sense if I already needed a live interactive demo — but I don't
have one yet, and taking on a framework's dependency management right now would cost me more
time learning tooling than building my actual portfolio. Plain HTML is something I can fully
read, edit, and fix myself, with nothing hidden — that matters more to me right now than
future flexibility I'm not using yet. I can maintain this stack myself, today, without
needing to ask for help every time something needs a small change. If the site grows a lot
bigger later, I may need to revisit this — but for four pages and one case study, it's the
right fit.
