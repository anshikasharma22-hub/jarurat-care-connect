# Jarurat Care Connect

A mini healthcare support web app built as a concept-level submission for the **Full Stack Developer (AI-enabled) Internship** assignment at Jarurat Care Foundation.

**Live demo:** _add your hosted link here after deploying_
**Repo:** _add your GitHub/Drive link here_

---

## What it does

One page, two purposes:

1. **Patient support request** — someone needing help (medicine access, treatment guidance, financial or emotional support) fills a short form describing their need.
2. **Volunteer registration** — someone who wants to help fills the same form, but it adapts to ask about their skill area and availability instead.

Both submissions are timestamped and stored (see "Storage" below), so the NGO team has a single, organized inbox of requests instead of scattered WhatsApp/email messages.

A floating **Care Assistant** chat widget sits on every page and answers common questions (how to volunteer, what support is offered, expected response time, data privacy) instantly, without needing a staff member online.

## Tech stack

- **Frontend:** Plain HTML, CSS, and JavaScript — no build step, so it can be deployed by dragging the folder into Netlify/Vercel with zero configuration and zero build-failure risk.
- **Storage:** Supabase (PostgreSQL), used via its REST API directly from the browser. `supabase-setup.sql` contains the table + row-level-security policy to create the backend in under a minute. If Supabase isn't configured, the form falls back to browser `localStorage` so the flow is still fully testable end-to-end without any setup.
- **AI / automation idea:** A rule-based FAQ chatbot (keyword-matching over a small intent list). It's intentionally simple for this concept stage, but the same interface is designed to be a drop-in point for a real LLM later — see below.

## The AI idea, and why it's built this way

The assignment asks for one AI/automation idea. I picked an **FAQ chatbot** because it directly reduces repetitive load on a small NGO team: most incoming questions ("how do I volunteer", "how long will you take to reply", "is my data safe") are predictable, and answering them instantly — before a human ever sees the message — means staff time goes to the requests that actually need a human judgment call.

For this concept submission the chatbot is rule-based (keyword matching against an FAQ array in `index.html`), so it's transparent, has zero running cost, and needs no API key to demo. The natural next step — and the reason the code is structured with a single `botReply()` function — is to swap that function for a call to the Anthropic API (Claude), sending the user's question plus the NGO's FAQ/context so it can handle open-ended phrasing instead of only exact keyword matches. That upgrade path is a few lines of code, not a redesign.

A second automation angle worth calling out: every submission could trigger an **auto-response email/SMS** ("we've received your request, expect a reply in 24–48 hours") the moment it's saved to Supabase, using a Supabase Edge Function or a simple webhook — the same pattern used elsewhere for order/status notifications. Not built into this concept version to keep the submission focused, but noted here as the logical next feature.

## NGO use-case

Jarurat Care Foundation deals with two very different audiences through the same intake problem: people who need help, and people who want to give their time. Today that likely happens over scattered channels (calls, WhatsApp, forms in different places). This app gives both audiences **one clear front door**, immediately tells them what happens next, and answers their obvious first questions without waiting on a person — while every submission lands in one structured table the team can act on.

## Running locally / deploying

No build step required.

- **Locally:** just open `index.html` in a browser.
- **Hosting:** drag the folder into Netlify, or run `vercel` in this directory, or connect the repo on Vercel/Render as a static site. Any of these will give the mandatory live link.
- **To store real submissions:** run `supabase-setup.sql` in a Supabase project's SQL editor, then paste the project URL and anon key into the `SUPABASE_URL` / `SUPABASE_ANON_KEY` constants near the top of the `<script>` in `index.html`.

## Files

- `index.html` — the entire app (markup, styling, and logic)
- `supabase-setup.sql` — optional table + RLS policy for live storage
- `README.md` — this file
