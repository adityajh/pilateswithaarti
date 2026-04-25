# BodyTalk × Pilates with Aarti — brand discovery questionnaire

A static, single-file discovery questionnaire for Aarti's Phase 1 brand reinvention. Hosted on Vercel; submissions POST to a self-hosted n8n workflow which formats and emails the response.

## Stack

- **Frontend** — Single `index.html` (~113 KB). Vanilla JS, inline CSS. No build step.
- **Fonts** — Cormorant Garamond + Inter from Google Fonts.
- **Persistence** — `localStorage` per browser session, with a save-and-resume URL.
- **Submit** — POST JSON to n8n webhook → Code-node JS formatter → Resend email.

## Local development

```bash
# Just open index.html in a browser, or:
python3 -m http.server 8000
# Visit http://localhost:8000
```

The submit endpoint is set to the production n8n webhook by default; submissions in dev will trigger real emails. To dry-run, change `CONFIG.webhookUrl` in `index.html` to a string containing `CHANGE-ME` — the form will log to console instead of POSTing.

## Deployment

Vercel:

1. Connect this repo to a Vercel project.
2. Framework preset: **Other** (or "No framework").
3. Root directory: `./` (default).
4. Build command: leave blank.
5. Output directory: `./` (default).
6. Deploy.

The form is fully static — Vercel serves `index.html` and `images/` directly.

## Endpoint configuration

```js
// in index.html, near the top of <script>
const CONFIG = {
  webhookUrl: 'https://n8n-5oud.srv1391034.hstgr.cloud/webhook/aarti-brand-discovery',
  storageKey: 'aarti-brand-discovery-v1',
};
```

## Repository structure

```
.
├── index.html                       # the form (single-file)
├── images/
│   ├── moodboard/
│   │   ├── W1..W5/
│   │   │   ├── w{n}-tearsheet.jpg   # composed editorial moodboard per world
│   │   │   └── w{n}-{01..05}.jpg    # individual reference frames
│   │   └── manifest.json
│   └── photo-categories/
│       └── cat-{1..8}.jpg           # §4 photography reference tiles
├── README.md
└── vercel.json
```

## Phase 1 context

Built as part of the Phase 1 brand-discovery workstream. Spec lives in the project's `brand-memory/` folder (not in this repo). The questionnaire's content is anchored to:
- The 2022 Enterprise India Fellowship harvest (Janhavi, Saneya, Ayesha)
- Aarti's own 2022 founder interview
- The confirmed credential stack (Balanced Body, Michael King, APPI, Moushumi apprenticeship, Breathe Education's Diploma of Clinical Pilates 2024-25, NOI Group's Explain Pain + Bodily Relearning)

## Submission flow

```
Form submit
    │
    ▼
n8n webhook  POST /webhook/aarti-brand-discovery
    │
    ▼
Code node (Format Email)  →  builds structured HTML email from payload
    │
    ▼
Resend HTTP node  →  sends to adityaj@adipa.com
```

n8n workflow ID: `qRvRV52RYsTXIDCv`.
