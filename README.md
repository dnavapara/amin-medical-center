# Amin Medical Center — Website

A production-ready site for Amin Medical Center (internal medicine, Skippack & Lansdale, PA) with three live AI features powered by Claude:

- **Care assistant** — answers practice FAQs, light triage, emergency escalation to 911 language
- **Smart scheduling** — patient describes their need, AI recommends visit type/duration/urgency, then an appointment request goes to the office
- **AI intake** — free-text note structured into a clinician-ready pre-visit summary

## Project structure

```
amin-site/
├── index.html      # The complete website (HTML + CSS + JS, no build step)
├── api/
│   └── chat.js     # Serverless proxy to the Anthropic API (keeps the key server-side)
└── README.md
```

## Deploy (Vercel — ~5 minutes)

1. Create a free account at vercel.com and install the CLI: `npm i -g vercel`
2. From this folder, run `vercel`
3. In the Vercel dashboard → Project → Settings → Environment Variables, add:
   - `ANTHROPIC_API_KEY` = your key from console.anthropic.com
4. Redeploy (`vercel --prod`). Done — the AI features are live.

Netlify works too: move `api/chat.js` to `netlify/functions/chat.js` and adjust the fetch path in `index.html` from `/api/chat` to `/.netlify/functions/chat`.

## Before going live — checklist

- [ ] Confirm the provider roster with the office (is Dr. Bipin Amin still seeing patients? Exact NP names?)
- [ ] Replace `appointments@aminmedicalcenter.com` in `index.html` with the practice's real scheduling email
- [ ] Verify hours for both offices (listings online differ slightly)
- [ ] Confirm the insurance list with the office
- [ ] Add the practice's patient portal link (Medent portal) to the nav if desired
- [ ] Point the practice's domain at the deployment (Vercel → Domains)
- [ ] Have the practice review the AI assistant's answers and the medical disclaimer with their compliance/legal advisor

## Notes

- Appointment "slots" are AI-recommended suggestions; the actual booking is a request the office confirms (via the email handoff). For true real-time booking, integrate the practice's scheduler (e.g., Medent patient portal API or a service like NexHealth).
- The API proxy caps message history and length, and never exposes the API key to the browser.
- If the AI backend is unreachable, every feature degrades gracefully to phone numbers.
