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
