# Platform recovery patterns

## Xiaohongshu / Rednote

1. Open the share short link in a real browser.
2. Preserve the complete redirected `/explore/<note_id>` URL, including signed query parameters.
3. Retry the available downloader with the resolved URL.
4. Read caption, creator identity, images/video, and author replies.
5. For image posts, OCR every tutorial image.
6. For short videos, extract approximately 1 frame/second; for longer videos begin around 1 frame/2 seconds, then densify around brief overlays.

Treat platform/AI comment summaries as discovery aids, not source facts, until values are verified against creator content.

## Douyin / TikTok China

A short link may resolve to a JS/encrypted shell or slides/note page. Public HTTP requests can return empty or anti-bot responses.

Preferred order:

1. use a logged-in browser/downloader;
2. inspect downloaded metadata JSON/chapter summaries;
3. isolate the intended creator/title/item ID when share text contains multiple links;
4. OCR downloaded slides first for image notes;
5. for video, combine chapter data, dense frames, and transcript.

If login/cookie/media access is unavailable, request screenshots or the saved video. Do not generate a final recipe from the share caption alone.

## Honest fallback

If only partial evidence is available, create an evidence-limited structured note with `待补 OCR/待确认` fields. Do not present it as a finished source-faithful card.
