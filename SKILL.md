---
name: remotion-trending-douyin-pipeline
description: End-to-end workflow for automatically selecting GitHub Trending or hot AI projects, generating Chinese Remotion short videos, creating 3:4 and 4:3 Douyin covers, uploading to Douyin Creator Center, publishing when explicitly requested, and recording the result.
metadata:
  tags: remotion, douyin, github-trending, video, cover, upload, publish
---

# Remotion Trending Douyin Pipeline

Use this skill when the user asks to run or build the full short-video pipeline:
find a trending/open-source AI project, generate a Chinese Remotion video,
make Douyin covers, upload, publish, or maintain a repeatable publishing loop.

For the full operating protocol, load [references/pipeline.md](references/pipeline.md).

## Default Contract

Produce a dated delivery folder:

```text
out/<project-or-topic>-YYYY-MM-DD/
```

Final artifacts:

- `*-video.mp4` final Remotion render with Chinese voiceover.
- `douyin-cover-3x4.png` vertical cover, `1200x1600`.
- `douyin-cover-4x3.png` horizontal cover, `1600x1200`.
- `douyin-title.txt` platform title only.
- `douyin-platform-copy.txt` title, description, and hashtags only.
- `source-data.json` checked source data and generation notes.
- `publish-qc.md` final verification and upload/publish evidence.

## Quick Workflow

1. Select a current candidate from GitHub Trending, GitHub search, Hacker News,
   Product Hunt, or explicit user sources. Use current data, not memory.
2. Run the duplicate gate against the local video library before committing to
   a topic.
3. Research README, releases, docs, screenshots, usage, stars, license, and
   risk/disclaimer needs. Save exact source links and checked date.
4. Create a plain Mandarin short-video script: hook, problem, project value,
   workflow/demo, caveat, call to action.
5. Build the Remotion composition with visuals matching each narration segment.
6. Generate voiceover, captions, and final MP4.
7. Generate both Douyin covers. Prefer AI-generated no-text backgrounds plus
   local exact Chinese text overlays.
8. Run QC before upload: lint/build where available, ffprobe duration alignment,
   full playback, frame/contact-sheet review, cover dimensions, text legibility.
9. Upload with browser automation only after QC is `PASS`.
10. Stop before public publish unless the user explicitly asked to publish. If
    publishing is explicit, confirm both covers are uploaded and no platform
    warning or verification gate remains before clicking publish.
11. Update the video library with selected topic, output folder, source links,
    upload state, publish state, and any blocker.

## Non-Negotiables

- Do not publish publicly without explicit user wording such as "发布" or
  "publish it".
- Never bypass Douyin login, QR, captcha, SMS, face verification, or policy
  warnings.
- Do not upload if `publish-qc.md` lacks `PASS` for audio/video duration
  alignment and narration-to-visual fit.
- Do not trust AI-rendered Chinese text. Overlay exact Chinese text locally and
  visually inspect the final PNGs.
- Do not include source URLs inside Douyin platform copy unless the user asks
  for links there. Keep links in `source-data.json` or `publish-qc.md`.
- For finance, security, medical, legal, or investment-related projects, add a
  clear research-only / not-advice disclaimer in narration and copy.

## Common Commands

```powershell
npm run lint
npx remotion still <composition-id> --frame=60 --scale=0.5 <output-folder>/still-check.jpg
npx remotion render <composition-id> <output-folder>/<name>-video.mp4
ffprobe -v error -show_entries format=duration -show_entries stream=index,codec_type,codec_name,width,height,sample_rate,channels -of json <output-folder>/<name>-video.mp4
```

Use Playwright or the available browser tool for Douyin Creator Center upload:

```text
https://creator.douyin.com/creator-micro/home
```

