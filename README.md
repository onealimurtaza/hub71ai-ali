# Arrival — Ali | Relocating to Abu Dhabi

Hub71+ AI Hackathon · 2 October 2026 · repository name: `hub71ai-ali`

Arrival helps individuals, families and companies turn a move to Abu Dhabi into a clear, phased plan. The interface starts with a message and a microphone button. It asks one question at a time, shows the emerging profile, lets the user correct it and generates a relevant checklist with official service links.

## Demo

https://arrival-abu-dhabi.hub71-hackat-7654.chatgpt.site/

The current published demo is owner-private. Judge access must be arranged before submission. The previously deployed version uses guided logic. The prepared server upgrade uses OpenAI when `OPENAI_API_KEY` is configured; until that is configured and the upgrade is deployed, do not describe the live app as AI-powered.

## What OpenAI does in the prepared upgrade

The server calls the Responses API with Structured Outputs to extract multiple facts from natural-language answers, update a structured relocation profile and phrase one relevant follow-up question. The browser never receives the API key. The default model is `gpt-4.1-mini`; `OPENAI_MODEL` can override it. The model does not make visa eligibility decisions or invent fees. Plans come from the reviewed task rules in `web/relocation-engine.js`.

If the key is missing or an API call fails, the interface explicitly identifies guided mode. Do not claim a successful live AI call based on simulated tests or the presence of a configured secret.

## Run locally

Requires Node.js 20 or later. No dependencies need to be installed.

```sh
node --env-file=.env scripts/serve.mjs
```

Copy `.env.example` to `.env` and configure your own server-side OpenAI credentials. Without credentials, guided mode remains available. Open http://localhost:3000. This local server is a development fallback, not the preferred judged deployment.

## Verify and build

```sh
npm test
npm run build
```

The deterministic build embeds public web assets and the shared task rules into `dist/server/index.js`, a Cloudflare-compatible Worker exporting `fetch(request, env, ctx)`. The source of truth is `web/` and `server/`, not generated output. The `.openai/hosting.json` identifies the existing Sites project. Set runtime secrets through Sites, then publish the matching saved version. Never put credentials into the manifest or a GitHub commit.

## Structure

- `web/`: conversational frontend, voice-to-text integration, full planner and budget.
- `web/relocation-engine.js`: eight user paths and reviewed task-selection rules.
- `server/worker.mjs`: same-origin API, validated structured profiles, bounded inputs, timeouts, explicit error handling and a basic per-isolate burst limit.
- `scripts/build.mjs`: deterministic Worker build with embedded assets.
- `scripts/serve.mjs`: local server for development.
- `test/worker.test.mjs`: API contract, failure, privacy, branch and guided-mode checks using simulated API responses.
- `data/relocation-tasks.json`: reproducible snapshot of the actual rule engine, with source links and applicability examples.
- `DEMO.md`: one three-minute household journey.
- `SUBMISSION.md`: fields to complete at final submission.

## Differentiation and dataset

A household is not a single visa: an employed adult, a partner starting an online business, a toddler and elderly parents need different steps. Arrival coordinates those needs in one plan. The dataset is a curated, structured task catalogue assembled from public official guidance; it is not proprietary government data or a live eligibility database. Review dates indicate the demo research date, not a guarantee that rules remain unchanged.

## Limits

No government submissions, appointments, bank approvals, payments or property transactions occur. Voice input depends on browser support and permission; dictated text is reviewed before sending. Local progress is stored only in the current browser. AI-enabled messages are sent to OpenAI with `store: false`; this is not a claim of zero retention. Sensitive document uploads are not supported. The rate limiter is per Worker isolate and is not a globally enforced spending cap. A public production launch needs stronger abuse protection and ongoing source review.

## Final submission freeze

Target: submit by 15:00 Asia/Dubai on 2 October 2026. Organiser hard deadline: 15:45. Record the GitHub commit SHA, matching deployment, access instructions and verification results. After final submission, do not alter the judged code, prompts or UI. Keep any later work in a separate development branch and outside the judged deployment.
