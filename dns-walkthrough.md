# DNS Walkthrough — What Happens When Someone Visits My Site

## What a CNAME Record Is

A CNAME record is a DNS instruction that says: "this address is just another name for a
different address — go look there instead." It doesn't hold an actual server location
(an IP address) itself; it just points forward to another domain name, which is where the
real address lookup finally happens.

When my FlyRank subdomain (`lanreodesanya.flyrank.ai`) is provisioned, Ops will create a
CNAME record for it. That record's value will be my Netlify site's address —
`lanreodesanya.netlify.app` — so the record effectively says: "for `lanreodesanya.flyrank.ai`,
go check `lanreodesanya.netlify.app` instead." Netlify's own servers are what actually
answer once the lookup lands there.

## What Actually Happens Between Typing an Address and the Page Loading

Say someone types `lanreodesanya.flyrank.ai` into their browser. Here's the real chain of
events, plainly:

1. **Your browser asks a resolver.** A resolver is a middleman server (often run by your
   internet provider, or a public one like 1.1.1.1) whose whole job is to go find the
   answer on your behalf. Your computer doesn't know the answer itself — it just asks.

2. **The resolver checks if it already knows.** If someone else recently asked for the same
   address, the resolver may have the answer cached already, and can skip straight to
   answering. If not, it starts asking around.

3. **The resolver works its way down, step by step.** It first asks a root server "who
   handles `.ai` domains?" Then it asks that `.ai` server "who handles `flyrank.ai`
   specifically?" That server (the **authoritative nameserver** for `flyrank.ai`) is the
   one that actually holds FlyRank's real DNS records — including the CNAME record for my
   subdomain.

4. **The authoritative nameserver answers with the record.** For
   `lanreodesanya.flyrank.ai`, it returns the CNAME record's value:
   `lanreodesanya.netlify.app`. That's not a final answer yet — it's a redirect to another
   name.

5. **The resolver follows that redirect.** It now has to look up
   `lanreodesanya.netlify.app` the same way, which eventually leads to Netlify's actual
   servers and their IP address.

6. **The resolver hands the final answer back to your browser.** Your browser now knows
   which real server to actually connect to, and requests the page from it.

7. **The server responds, and the page loads.** Netlify's server sends back my site's
   files, and the browser renders the page — the whole process most people experience as
   just "typing an address and having it work," even though it's really several
   handoffs happening in a fraction of a second.

## Why I'm Writing This Before I Need It

My FlyRank subdomain isn't provisioned yet — it happens later, once my capstone is
approved. When it is, Ops creates the CNAME record on their end, and my job is the other
half: add the custom domain in Netlify's settings, point it correctly, wait for the record
to propagate, and confirm the HTTPS padlock actually appears. This walkthrough is the
checklist I'll run at that point — writing it now, while it's fresh, means I won't be
learning DNS for the first time under time pressure later.
