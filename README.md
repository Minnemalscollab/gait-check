# Minnemals Gait Check — educational trial

Standalone static site. No build, AI service, Claude login, backend, or external runtime CDN is required. All tracking scripts/models are in `mp/`. Serve over HTTP locally and HTTPS publicly, not `file://`.

    python3 -m http.server 8765 --directory /workspace/minnemals-gait

Local URL: http://localhost:8765/

## Access and flow

The single editable constant in `index.html` is:

    const ACCESS_CODES = ['GAITCHECK'];

The gate ignores case and surrounding spaces. It is **client-side only**, not secure authentication, buyer verification, or a secret. Anyone with the source can learn or bypass it.

Default: upload/record **Front, Back, Left side, Right side**. Analyze stays disabled until all four are supplied. All four must yield usable tracking before proceeding to the seven foot checks. A failed required clip stays on Film, naming the view and giving targeted refilm guidance. The quiet “Can't film?” alternative requires confirmation and opens the movement assessment only. Those reports explicitly have no walk data. All-skipped movement-only reports never claim good results, prescribe drills, or save an empty baseline.

## How the observations and plan work

- MediaPipe produces pose landmarks locally. These are rough phone-image measurements, not a clinical gait exam or diagnosis. The original numeric heuristics have NOT been clinically validated for this app.
- Each clip needs at least 30 sufficiently visible frames, at least 2.5 seconds of usable tracking, no long tracking gap, adequate person size, and no large position jump. Side views also need four alternating step events with plausible estimated cadence. Front/back need visible alternating foot movement. These are conservative engineering guards, not validated confidence scores.
- Side estimates use foot-separation peaks, not reliably identified heel-strike events. Report language calls these spacing/slope estimates rather than claiming true contact timing. Uncalibrated toe-out angles are excluded from trial recommendations. Footwear, clothing, speed, camera pan/angle, and occlusion can change estimates.
- The report leads with a reading for each of the four labeled clips, then the walk cues and foot results. Anatomical left/right is estimated by the tracker. **Camera angles and subject identity are not independently verified**; use one person and a fixed camera. Selecting a slot does not prove the correct view was filmed.
- Skipped foot answers are unknown, never a pass. Responses 0/1/2 describe comfortable both sides / one side harder / both sides harder, not medical severity. Toe results combine four relevant checks; the other areas use their own checks. Partial coverage is labeled.
- If a video cue is triggered, Walk is the first priority. Remaining difficulty areas are ordered by reported extent, then toes, arch, ankle, balance. At most three priorities are displayed. Comfortable areas get maintenance; missing information gets observation/retest tasks. No problem is invented to fill three slots.
- Each selected practice has a dose, simple instructions, and a progress cue. The five-minute routine uses smaller doses and replaces, rather than adds to, the full-dose drills. Walking cues are gentle exploration, never a claim to fix an injury or explain its cause.
- The first nonempty report saves a baseline; subsequent reports do not overwrite it. Quick retest compares paired foot responses, NOT a new walk or a clinically measured improvement. Retest date is baseline +14 calendar days; checklist Day 1 is the following day.
- Missing/partial data or any difficulty gives the slower optional Minnemals wear range. Neither range proves tissue readiness. Only increase after two comfortable days and next mornings; familiar shoes cover the rest of the day. Already adapted wearers keep their tolerated amount.
- The safety flag suppresses exercises and footwear scheduling and recommends individual professional advice. Do not test or push through pain, numbness, recent injury, diabetes/neuropathy, or uncertainty.

## Privacy and persistence

Videos are processed in browser memory and not uploaded or stored as video files. **Report still images, answers, baseline, and checklist ARE saved in this browser's localStorage** (`minnemals-gait-v3`). Older prototype saved reports are not imported, because they can contain incomplete or simulated walk data. Clearing saved results removes the current record and older v2 record. Browser storage can be cleared; there are no accounts, cross-device sync, or recovery service. If storage is unavailable/full, an in-memory fallback lasts only while the page stays open. Print/PDF or copy for a portable record.

No production test hook, debug query route, or sample metric injection remains. Reproducible QA lives outside the deployed directory, in `/workspace/minnemals-gait-qa/`. Its synthetic tests intercept HTML only in their own browser session; they never alter the shipped site.

## Lasting hosting — not yet deployed

Use an **account-owned Netlify project**, not an anonymous Drop/preview link:

1. Delaney signs into https://app.netlify.com/login and selects the intended team.
2. After explicit approval to deploy: Projects → Add new project → Deploy manually; upload this complete directory (with `index.html` at its root and `mp/` beside it).
3. Verify project/site visibility permits trial participants and confirm the stable `*.netlify.app` URL. Update that same project for future versions. No build command is needed. Standard static hosting serves `.wasm` as `application/wasm`.

No Netlify connector is installed in this bot. Catalog Netlify plugin 20992516 contains skills, not account-access tools. Installing that plugin is NOT a Netlify login. The official Netlify guide also describes authorization through Grok Connectors if that integration is available in the user's app.

Official references:
- https://docs.netlify.com/manage/projects/add-new-project/
- https://docs.netlify.com/build/build-with-ai/agent-setup-guides/set-up-grok-for-netlify/

**No deployment or Shopify change was performed for this audit. Keep the Shopify Gait Check page unpublished.** Later, an approved Shopify/email link can point to the hosted standalone site. Pasting this HTML into Shopify's rich-text editor does not host the local model assets.

## Trial limitations and owner decisions

This is an educational prototype, not clinically validated personalized treatment. Before inviting a wider audience:

- Try an actual matched set of all four views on physical iOS Safari and Android Chrome phones. Desktop Chromium at 390px is not physical-phone testing. Processing may take a minute per view on slower devices; only the first 12 seconds are analyzed.
- Review all exercise doses, check wording, thresholds, and footwear starting suggestions with an appropriately qualified clinician.
- Confirm/replace the original Gait Happens practitioner directory; no endorsement or referral-network relationship is claimed.
- Confirm hosting/team and whether a shared convenience code is sufficient. Decide whether browser-only persistence meets the trial's needs.
- Optionally supply official vector logo/fonts. Navy, cream, forest, mint, and the wordmark derive from the export; copy was informed by Minnemals.com: roomy toe box, thin flexible sole, everyday minimalist movement, warm/direct tone. No sales or medical-outcome promises were added.

## QA evidence

Real execution test: a licensed public full-body side walking clip (Mixkit 4855) produced 90 tracked frames per run and 11 alternating step events (about 111 steps/min). The same side clip was deliberately placed in every slot for branch testing: side analysis succeeded, front/back correctly rejected the wrong angle, and the app stayed on Film. This is real algorithm execution, NOT a four-distinct-view success test or clinical validation. Multi-person surveillance footage also correctly failed quality guards. Successful complete four-slot routing/report generation was tested with synthetic landmarks in the isolated QA harness only.

Latest logs: `/workspace/gait-audit/`; latest screenshots: `/workspace/minnemals-gait-shots/`. See the dated audit summary there for exact real-versus-synthetic test outcomes. Any screenshot with a synthetic QA banner is a layout/routing test, NOT evidence of real-video or clinical accuracy. Earlier prototype QA scripts/logs were superseded and can rely on removed test hooks.

## Third-party files

MediaPipe Pose 0.5.1675469404, by Google, is bundled in `mp/`. Public source: https://cdn.jsdelivr.net/npm/@mediapipe/pose@0.5.1675469404/ . Apache-2.0 license: `MEDIAPIPE-LICENSE.txt`. SIMD and non-SIMD runtimes are local. Research/test videos are **not included in this deployment bundle**.
