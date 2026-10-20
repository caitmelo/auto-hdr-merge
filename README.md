# Automatic HDR Merge — independent copy

Complete HDR Studio source: RAW grouping, thumbnails, alignment, automatic motion suppression, numerical HDR merge, local development, optional GPT Image finishing and batch exports. Prepared from public source commit `779d6d9ff057e59d2630e850e608ded6e4d26b78` for a separate repository.

## Manual-lens correction

Confirmed the original exposure function returned null without a positive aperture. It now uses log2(shutter × ISO) when aperture is absent/zero/invalid. With valid aperture, the original log2(shutter × ISO / aperture²) calculation remains. Valid shutter and ISO are still required.

The fallback assumes a fixed physical aperture and consistently missing aperture metadata throughout the bracket. Do not mix known-aperture and unknown-aperture scales in one group; enter relative stops manually for inconsistent metadata or an unrecorded aperture change.

Validation: 37 passing tests, including a simulated three-shot Pentax-labelled bracket with missing aperture, correct −2/0/+2 stop spacing and recovery of unclipped scene values. Actual Pentax RAW files were not supplied for independent decoder validation.

## Build and test

Run `npm test`, then `npm run build`. No npm dependencies are required. Build emits `dist/server/index.js` for a compatible Worker runtime. The original hosting project binding is omitted: create a new hosting project for this copy. Configure its origin and verified authentication in `src/server.js`; set an OpenAI server secret only if AI finishing is needed. Never expose that secret in browser code.

See `DEVELOPER_HANDOFF.md` and `docs/AI_PROMPT.md` for inherited processing/API details. Original app/repo links in those historical notes describe provenance, not a deployment of this copy. Neither the original repository nor the original live app has been changed.

## Core functions

`public/engine.mjs` contains the numerical HDR functions. `public/process.worker.mjs` orchestrates RAW decoding and merging; `public/app.js` handles grouping, review and the queue. `public/render.mjs` handles local tone rendering. `src/server.js` handles the optional single GPT Image finish per group. All source files and tests are included.
