# Make It Do Something — Contact Form Explainer

## The feature

My portfolio's Contact page now has a real, working form (name, email, message) instead of
a plain "click to open your email app" link. When someone submits it, the message actually
reaches my inbox — no email client required on their end, and I get a record of it.

## What a "backend" actually means here

A backend is just the part of a system that runs somewhere other than the visitor's own
browser — code or a service that receives data, does something with it, and sends a
response back. My site itself (the HTML, CSS, and the little bit of JavaScript on the
Contact page) is the **frontend** — it only runs on the visitor's device and can't send an
email on its own. GitHub Pages, which hosts my site, only serves files; it has no way to
process a form submission or send mail.

That's why I used **Formspree**: it's a small hosted backend I don't have to build or run
myself. It exists specifically to receive form submissions from static sites like mine and
forward them as email.

## How the data actually flows

1. A visitor fills in the form on my live Contact page and clicks Send.
2. My page's JavaScript catches that click, packages up the three fields (name, email,
   message) into a request, and sends it — not to my own site, but directly to Formspree's
   servers, at a unique URL Formspree gave me when I created the form.
3. Formspree receives that data, checks it isn't spam, and — this is the actual "backend"
   work — formats it into an email and sends it to my real inbox.
4. Formspree sends a response back to my page saying it worked.
5. My page's JavaScript reads that response and swaps the form out for a "Thanks — that
   reached me" message, without reloading the page.
6. Separately, and completely outside anything my site controls, that email physically
   arrives in my Gmail inbox a few seconds later.

## One small deliberate detail

There's a hidden field on the form (`_gotcha`) that a real visitor never sees or fills in.
Automated spam bots that blindly fill in every field on a page often fill it in anyway —
so if that field ever arrives non-empty, Formspree quietly discards the submission instead
of emailing it to me. It's a simple, free way to filter out obvious spam without needing any
real security system.
