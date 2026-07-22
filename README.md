<<<<<<< HEAD
# Creator Casting Pipeline

An n8n automation that handles applicant intake for a YouTube creator running three casting-call formats — **Pop the Balloon**, **Find Your Match**, and **Who is the Liar**. It takes a webform submission all the way through eligibility screening, AI scoring against a show-specific rubric, duplicate-identity detection, and producer-ready shortlisting — so a human only has to review a filtered, ranked list instead of every raw application.

**Walkthrough (Loom):** https://www.loom.com/share/79ef3918987a45ed9b2cd5e0c885fc36

## What it actually does

- Custom webform with **per-show dynamic questionnaires** — the questions themselves change based on which show the applicant picks, since a dating-elimination show and a deception game aren't looking for the same thing.
- **Hard eligibility gate** before anything costs an API call: valid email, a working audition video link, actual age from date of birth (not a self-reported checkbox), and a filming-location whitelist.
- **Real duplicate-identity detection** — checks a new applicant's phone/Instagram/TikTok against everyone already logged, not just their email, and flags (doesn't auto-reject) likely repeat identities for a producer to judge.
- **Show-specific AI scoring** — each show gets its own rubric (charisma/wit for Pop the Balloon, emotional intelligence for Find Your Match, composure/bluffing for Who is the Liar), not one generic prompt applied to everyone.
- **Red flags always force human review** — a high AI score can't auto-promote someone to the instant-alert shortlist if the model also flagged a concern.
- Routes each applicant into their own show's tab, plus a cross-show master log, with a `Status` column producers fill in themselves (`Needs Review` → `Shortlisted` / `Rejected` / `Booked`). The automation ranks and groups; it doesn't decide.
- Instant Slack + Telegram alert only for the top tier (`Strong Shortlist`) — everything else waits quietly in the sheet instead of pinging producers for every applicant.
- Separate error-handler workflow catches genuine system failures (API down, credentials expired, malformed payload crashing a node) and pings Telegram — distinct from routine applicant disqualifications, which just log silently.

## Repo contents

| File | What it is |
|---|---|
| `creator-casting-pipeline.json` | Main n8n workflow — import this first |
| `casting-pipeline-error-handler.json` | Separate error-handling workflow — import second, then point the main workflow at it in Settings |
| `casting-application.html` | The applicant-facing webform (self-contained HTML/CSS/JS, no build step) |
| `WORKFLOW.md` | Architecture diagram, design decisions, and full bug log from building this |

## Setup

1. **Import both n8n workflows**, error handler first (see `WORKFLOW.md` for why the order matters).
2. **Create one Google Sheet with 8 tabs**, headers matching exactly (case-sensitive) — full column lists are in `WORKFLOW.md`:
   `Master_Log`, `Pop_the_Balloon`, `Find_Your_Match`, `Who_is_the_Liar`, `Other_Formats`, `Disqualified`, `Needs_Video_Fix`, `Error_Logs`
3. **Wire credentials** on every node: Google Sheets, Gmail, OpenAI, Slack (Bot Token, `chat:write` scope, bot invited to the alert channel), Telegram.
4. **Replace every `YOUR_GOOGLE_SHEET_ID` / `YOUR_SLACK_CHANNEL_ID` / `YOUR_TELEGRAM_CHAT_ID` placeholder** with your real values.
5. On every "Append or Update Row" node, **manually reselect the Document, Sheet, and matching column** after import — this doesn't survive JSON import cleanly and needs to be reselected once a real sheet is connected.
6. Open `casting-application.html`, set `CONFIG.DEMO_MODE = false` and `CONFIG.CASTING_WEBHOOK_URL` to your live n8n webhook URL, then host it (Vercel, GitHub Pages, wherever it needs to live).

## Known limitations

- The duplicate-identity check reads the entire `Master_Log` sheet on every submission. Fine at low volume; will slow down and eventually hit Sheets API quota at high volume. Airtable or a real database would scale better if this ever gets busy.
- Social handles (Instagram/TikTok) are captured as self-reported text, not live-verified — Instagram in particular blocks unauthenticated scraping, so there's no reliable way to check follower counts without an official API partnership.
- The "Maybe" bucket has no digest yet — it just sits in the sheet for a producer to check manually. A weekly digest (same shape as an existing Friday-digest pattern from another project) is a natural next addition.
- Google Drive link validation is a HEAD request checking for a `200`, not a true Drive API permissions check — reliable for the common case, not bulletproof for every edge case Drive's sharing UI allows.
=======
# NexterTV — Casting Call Frontend

A high-performance, responsive static web application built for reality TV casting calls. This interface features dynamic per-show questionnaire switching, live validation, and asynchronous JSON payload submissions directly to an n8n backend automation pipeline.

## 🎬 Featured Shows

The platform supports three distinct casting tracks, each dynamically rendering tailored questionnaire fields upon selection:
1. **Pop the Balloon** — Fast-paced, blind-elimination dating format.
2. **Find Your Match** — Compatibility-focused, slow-burn emotional format[cite: 1].
3. **Who is the Liar** — Social deception and nerve testing game[cite: 1].

---

## 🛠️ Tech Stack

* **Markup/Styling:** Semantic HTML5, CSS3 with custom design tokens, Google Fonts (*Bebas Neue*, *Space Grotesk*, *Space Mono*).
* **Scripting:** Vanilla JavaScript (ES6+), asynchronous `fetch` API, dynamic DOM node injection.
* **Hosting / Deployment:** Optimized for static hosting providers like Vercel or Netlify.

---

## ⚙️ Configuration & Webhook Setup

To hook this frontend up to your live n8n workflow or ngrok tunnel, locate the `CONFIG` object near the bottom of the script tag in `index.html` and update it[cite: 1]:

```javascript
const CONFIG = {
  DEMO_MODE: false, // Set to false to push live data to your webhook
  CASTING_WEBHOOK_URL: "[https://your-n8n-instance-url.com/webhook/casting-application](https://your-n8n-instance-url.com/webhook/casting-application)", // Your active n8n or ngrok production URL
};
>>>>>>> 045364b8df691662ae06d096a29c3eda0534c013
