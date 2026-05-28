---
name: remotion-trending-douyin-pipeline-reference
description: Detailed operating protocol for the Remotion Trending to Douyin publishing pipeline
---

# Detailed Pipeline

## 1. Candidate Discovery

Use current sources every run:

- GitHub Trending page by language/topic.
- GitHub Search API or web search for recently popular AI, agent, MCP,
  developer-tool, productivity, data, or design repositories.
- Hacker News, Product Hunt, X/Twitter, Reddit, or user-provided sources when
  relevant.

Record in `source-data.json`:

- `checked_at`
- project name and repository full name
- source URLs
- stars, forks, language, license, topics, homepage
- latest release, changelog, or recent commit signal
- why the project is interesting now
- selected video angle
- duplicate-gate decision

Selection rules:

- Prefer projects that are currently gaining attention and can be explained in
  a short, useful Chinese video.
- Avoid repeating a previously published project unless there is a major new
  release, renewed viral signal, or a materially different angle.
- Avoid projects that cannot be shown with concrete visuals.

## 2. Video Library Gate

Maintain a local library such as:

```text
out/video-library.md
```

Before selecting the topic, search the library for:

- repository full name
- project name
- aliases
- category and angle

After upload or publish, append an entry:

```markdown
## YYYY-MM-DD | <project> | <status>

- Topic:
- Angle:
- Repository:
- Sources:
- Output folder:
- Douyin title:
- Status: drafted | uploaded | published | blocked
- Notes:
```

## 3. Research Packet

Read enough current material to explain the project accurately:

- README overview, install, usage, screenshots, architecture.
- Release notes or changelog.
- Docs or examples that demonstrate real usage.
- Issues/discussions only when needed for caveats.

High-stakes domains require clear disclaimers:

- finance/investing: research tool only, not investment advice
- security: test only in authorized environments
- medical/legal: informational only, not professional advice

## 4. Chinese Script

Use brisk, plain Mandarin for short video voiceover.

Recommended structure:

1. Hook: what changed or why it is useful.
2. Problem: what pain point it solves.
3. Project value: what the viewer can do with it.
4. Workflow/demo: show concrete usage or architecture.
5. Caveat: install complexity, maturity, safety, or domain limit.
6. Close: who should try it.

Avoid dense jargon. If a technical term is necessary, explain it immediately in
plain words.

Default voiceover preset:

- voice: `zh-CN-YunyangNeural`
- rate: `+40%`
- style: natural, conversational, low dramatic intensity

## 5. Remotion Build

Use a real-visual-first composition:

- GitHub page screenshot
- README feature screenshot
- release page
- docs/CLI usage
- app UI or demo
- simple architecture/workflow graphic when needed

Every narration segment needs a matching visual. Keep a scene mapping table in
`publish-qc.md`:

```markdown
| Start | End | Narration | Visual | Evidence |
| --- | --- | --- | --- | --- |
```

Recommended scene order:

- title/hook
- repository proof and current signal
- problem/solution
- workflow/demo
- quick start or usage
- caveat/disclaimer
- summary CTA

## 6. Covers

Create both files:

- `douyin-cover-3x4.png` at `1200x1600`
- `douyin-cover-4x3.png` at `1600x1200`

Preferred method:

1. Generate no-text AI background with GPT image / image2.0 / Ark:
   `no words, no letters, no UI text, no logos, no watermark`.
2. Build an HTML/CSS, Canvas, Pillow, or Remotion-still cover template.
3. Overlay exact Chinese text locally.
4. Screenshot/export at exact dimensions.
5. Open and inspect both final PNGs.

Cover content pattern:

- badge: domain or `GitHub Trending`
- main: project name
- subtitle: short Chinese value phrase
- metrics: stars, language, release/version, license when useful
- footer: source or safety note

Reject covers when:

- Chinese text is garbled, clipped, too small, or AI-generated pseudo-text.
- dimensions are wrong.
- the design is too busy for mobile.
- important text sits near Douyin crop/UI zones.

## 7. Pre-Publish QC

Do not upload until QC passes.

Minimum checks:

```powershell
npm run lint
npx remotion still <composition-id> --frame=60 --scale=0.5 <output-folder>/still-check.jpg
npx remotion render <composition-id> <output-folder>/<name>-video.mp4
ffprobe -v error -show_entries format=duration -show_entries stream=index,codec_type,codec_name,width,height,sample_rate,channels -of json <output-folder>/<name>-video.mp4
```

Duration alignment:

- Compare MP4 container, video stream, audio stream, Remotion duration, and
  narration file duration when available.
- Drift over `0.30s` fails unless intentionally documented intro/outro silence.

Narration-to-visual fit:

- Watch the final MP4 end to end with audio.
- Extract frames at scene boundaries, narration sentence boundaries, and at
  least once per second.
- Use a contact sheet for fast review.
- Confirm no blank, stale, unrelated, clipped, or misleading visual segment.

Write `PASS` or `FAIL` in `publish-qc.md` for:

- render success
- duration alignment
- narration-to-visual fit
- cover dimensions
- cover legibility

## 8. Douyin Upload and Publish

Open:

```text
https://creator.douyin.com/creator-micro/home
```

Workflow:

1. If login is required, stop for QR/SMS/captcha/face verification.
2. Click publish/upload video.
3. Upload the final MP4.
4. Wait until upload and processing reach the edit form.
5. Fill title, description, and hashtags from `douyin-platform-copy.txt`.
6. Upload the 4:3 horizontal cover.
7. Upload the 3:4 vertical cover.
8. Confirm both cover slots show the intended uploaded preview or selected
   state.
9. Confirm no spinner, retry button, crop-unsaved state, missing-cover warning,
   or platform error remains.
10. Record cover upload `PASS` in `publish-qc.md`.
11. Stop before public publish unless the user explicitly asked to publish.
12. If explicitly publishing, click publish only after checking warnings.

If a policy warning appears, summarize it. Continue only when the warning is
non-blocking and consistent with the user's explicit publish request.

## 9. Final Report

Report:

- output folder
- video path
- cover paths
- title/copy files
- selected source/project URLs
- upload/publish state
- QC evidence summary
- any blocker or platform warning

Use absolute filesystem paths for local artifacts.

