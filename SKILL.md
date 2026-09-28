---
name: recipe-video-cards
description: Use when a user sends a Xiaohongshu/Rednote, Douyin/TikTok, cooking-video link, screenshots, or a meal description and wants an evidence-grounded Chinese 图解食谱卡. This skill enforces source extraction, OCR/transcript verification, structured recipe data, a warm scrapbook golden layout, image-model generation, strict visual QA, and delivery of the actual PNG. Trigger for 图解食谱卡、食谱图、视频拆菜谱、低卡食谱卡、小红书同款食谱卡、把做饭视频做成图、golden recipe card, even if the user does not name the skill.
license: MIT
compatibility: Requires web/browser or downloaded-media access, frame extraction/OCR or multimodal vision, an image model that can render Chinese recipe cards (prefer GPT Image 2 or equivalent), and filesystem access for artifacts.
---

# Recipe Video Cards

Turn cooking links, screenshots, videos, or confirmed menus into reproducible Chinese illustrated recipe cards. Optimize for truthfulness first and visual quality second. A beautiful card with invented grams is a failure.

## Non-negotiable production contract

1. **Collect evidence before writing a recipe.** Resolve the real post, download or inspect the media, read the caption and author replies, extract frames, OCR brief overlays, and transcribe audio when useful.
2. **Separate evidence levels.** Mark facts as caption-confirmed, subtitle-confirmed, author-reply-confirmed, visibly shown, user-confirmed, or unknown.
3. **Never invent quantities.** If grams, counts, ratios, time, temperature, fire level, or calories are absent, write `原视频未标注` / `未给克数` / `适量`, rather than supplying a plausible number.
4. **Create structured data before image generation.** Save one recipe JSON per dish using `templates/recipe.schema.json` as the target shape.
5. **Generate one card per confirmed dish.** A multi-dish video requires multiple recipes/cards unless the user explicitly asks for one menu overview.
6. **Use the golden scrapbook layout.** Read `references/golden-layout.md` and, when the runtime supports reference-image editing, pass `assets/golden-reference.png` as the style/layout reference.
7. **Use a capable image model.** Prefer GPT Image 2 / image2 or another model proven to render Chinese. Do not silently replace the requested generative artwork with PIL, SVG, HTML, screenshots, or a generic template.
8. **Review the generated pixels.** Run multimodal visual QA against the exact recipe JSON and the golden-layout checklist. Regenerate on any critical content error, fake quantity, unreadable Chinese, clipping, ordinary-poster drift, or layout failure.
9. **Deliver the actual file.** Verify the final PNG exists, copy it to the user-facing output folder, and return/embed the image—not merely a path description or “done”.

## End-to-end workflow

### Phase A — Acquire the real source

- Preserve the original URL and the resolved post URL/ID.
- Capture title, creator, description, publication date, tags, media type, and author comments/replies.
- For short-link failures, follow `references/platform-recovery.md`.
- If media cannot be reached, ask for the saved video, screenshots, or exported images. Do not generate a final card from a title alone.

### Phase B — Build the evidence pack

For videos:

1. Inspect metadata/chapter summaries if available.
2. Extract coarse frames to understand the full timeline.
3. Run a dense pass around ingredient and cooking overlays (often 1 frame every 1–2 seconds).
4. Split frames into readable contact sheets.
5. OCR every sheet for grams, ml, counts, ratios, time, temperature, fire/oil temperature, resting, soaking, simmering, and doneness cues.
6. Transcribe speech if it contributes ingredients or technique.
7. Cross-check OCR, transcript, caption, author replies, and visible actions.

For image posts/screenshots:

- Enumerate every image.
- OCR tutorial images at full resolution.
- Do not infer method or amounts from a finished-dish photo.

### Phase C — Normalize the recipe

Create a JSON record containing:

- title, source URL/ID, creator, yield/servings;
- evidence summary and unresolved fields;
- ingredients grouped by component;
- seasonings/sauces separately;
- ordered executable steps;
- time, temperature, fire level, doneness cues;
- calories/macros only with an explicit basis;
- substitutions/tips only when source- or user-confirmed.

Calories stated by the creator must be labeled `作者口径`. Preserve yield and partial component use—for example, “一份做4个；麻薯整份只用一半；作者称约160 kcal/个”. Never imply independent nutrition verification.

### Phase D — Write the image prompt

Use `templates/image-prompt-template.md`. Include:

- exact title and all verified copy;
- grouped ingredient blocks;
- STEP 1–6 (or the minimum number that preserves the real method);
- hero-food description matching the actual finished dish;
- warm cream/parchment scrapbook layout;
- explicit negative constraints against fake quantities, six-grid/PPT drift, clipped text, gibberish, and text over food.

When source content exceeds one readable card, split it into multiple cards instead of shrinking text below legibility.

### Phase E — Generate

- Prefer portrait 2:3 or a similar long-card ratio.
- Supply the golden reference image when supported.
- Keep evidence logs and citations outside the public card. The card should contain cooking information, not internal extraction notes.
- Save the raw generated PNG in a work folder before delivery.

### Phase F — Strict visual QA

Read `references/qa-checklist.md`. Compare the image against both the structured JSON and the golden layout. Critical failures require regeneration:

- any wrong or invented quantity;
- missing component or meal item;
- missing time/temperature that is present in evidence;
- unreadable, clipped, overlapping, or corrupted Chinese;
- main food not visually recognizable;
- missing `STEP` sequence or kcal/yield badge when required;
- generic poster/PPT/grid rather than scrapbook recipe card;
- text covering the hero food.

Do not “explain away” a visible failure. The generated image is the deliverable and must pass on its own.

## Output artifacts

Recommended folder shape:

```text
recipe-output/
├── evidence/
│   ├── source-metadata.json
│   ├── transcript.txt
│   ├── frames/
│   └── contact-sheets/
├── structured/
│   └── recipe.json
├── prompts/
│   └── image-prompt.md
├── review/
│   └── visual-qa.md
└── final/
    └── 菜名-图解食谱卡.png
```

Final response should briefly identify the evidence basis, disclose important uncertainty or creator-calorie wording, and show/attach the final PNG.

## Runtime adaptation

Tool names differ across agents. Map capabilities rather than hard-coding one vendor:

- browser/search → resolve post and comments;
- downloader/HTTP → save media;
- ffmpeg or video tool → extract frames/audio;
- OCR/multimodal model → read overlays and review images;
- speech-to-text → transcript;
- image generator → render the card;
- filesystem/archive → save and deliver artifacts.

If the runtime lacks one capability, use the nearest honest fallback. Never claim media, comments, OCR, generation, or visual review occurred unless it actually did.

## Read on demand

- `references/golden-layout.md` — required visual standard.
- `references/evidence-pipeline.md` — detailed extraction and uncertainty rules.
- `references/platform-recovery.md` — Xiaohongshu/Douyin access fallbacks.
- `references/qa-checklist.md` — generation acceptance gate.
- `README.md` — installation and cross-agent usage.
- `evals/evals.json` — behavioral tests for a new agent install.
