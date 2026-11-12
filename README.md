# Nifty 50 SIP Investment Video — HyperFrames Project

An 85-second animated explainer video showing what happens when you invest ₹10,000 every year in the Nifty 50 index from 2006 to 2025. Built entirely with **HyperFrames** (HTML-to-video framework) + **Claude AI**.

## Final Output

| Version | Format | Resolution | Size |
|---------|--------|------------|------|
| `nifty50-sip-v5.mp4` | Landscape | 1920×1080 | 11 MB |
| `nifty50-sip-reels-v3.mp4` | Instagram Reels (vertical) | 1080×1920 | 6 MB |

## What the Video Covers

### The SIP Journey (₹10,000/year × 20 years)

| Year | Invested | Return | Portfolio |
|------|----------|--------|-----------|
| 2006 | ₹10,000 | +39.8% | ₹13,980 |
| 2007 | ₹20,000 | +54.8% | ₹37,121 |
| 2008 | ₹30,000 | **−51.8%** | ₹22,712 (GFC crash) |
| 2009 | ₹40,000 | +75.8% | ₹57,508 |
| ... | ... | ... | ... |
| 2025 | ₹2,00,000 | — | **₹7,61,175** |

**Result**: ₹2,00,000 invested → ₹7,61,175 (3.8× multiplier, 280.6% total return)

### Scenes Breakdown

| # | Scene | Time | Description |
|---|-------|------|-------------|
| 1 | **Hook** | 0–8s | Kinetic intro with floating particles, pulse rings, coin rain, chart silhouette, glitch lines |
| 2 | **Early Years** | 8–26s | 2006/2007 hero cards, 2008 GFC crash with screen shake and red flash |
| 3 | **Recovery** | 26–38s | Bar chart showing 2009–2014 comeback (+75.8% bounce) |
| 4 | **Compounding** | 38–53s | Line chart showing 2015–2021 exponential growth, COVID dip |
| 5 | **Summary** | 53–67s | Investment summary + lifestyle inflation comparison (2006 vs 2025 prices) |
| 6 | **Reveal** | 67–77s | Big portfolio reveal — ₹7,61,175 in a yellow sun circle |
| 7 | **Close** | 77–85s | CTA — "Start early. Stay consistent." with key stats |

## Tech Stack

### Core Framework
- **[HyperFrames](https://hyperframes.studio)** v0.9.38 — HeyGen's HTML-to-video framework
  - Each scene is a standalone HTML file with GSAP animations
  - Rendered to MP4 via headless Chrome screenshot capture at 30fps

### Animation
- **GSAP 3.15** — Timeline-based animations with custom easing
- **stepEase()** — Custom stepped easing function for that snappy motion-graphics feel:
  ```js
  const stepEase = (base, dur, fps = 12) => {
    const e = gsap.parseEase(base);
    const n = Math.max(1, Math.round(dur * fps));
    return (p) => e(Math.floor(p * n) / n);
  };
  ```

### Voice & Audio
- **Kokoro TTS** — Local text-to-speech via `npx hyperframes tts`
  - Voice: `am_adam` at 1.15× speed
  - Requires: `pip install kokoro-onnx soundfile`
- **SFX**: 18 bundled sound effects (whoosh, pop, ping, impact, glitch, etc.)
- Audio ducking: music at ~0.14 volume, SFX at 0.15–0.3 under voice

### Design System (Vox Palette)
- **Fonts**: Archivo Black (headlines), Inter (body), Space Mono (data/monospace)
- **Colors**: Ink #1A1A1A, Yellow #FFE619, Green #22c55e, Red #ef4444, White ground #fbfaf8

### Instagram Reels Conversion
- Vertical format via ffmpeg: blurred background + centered foreground overlay
- Subtitles burned in via `.srt` caption file
- Cover frames extracted at key moments

## Project Structure

```
nifty50-sip/
├── index.html              # Root composition — scene timing, audio tracks
├── hyperframes.json        # HyperFrames project config
├── narration_script.txt    # Full voiceover narration text
├── captions.srt            # 38 timed subtitle phrases
├── thumbnail.html          # Custom thumbnail design (rendered to JPG)
├── reels-optimization-guide.md  # Instagram posting guide
│
├── compositions/           # Scene HTML files (one per scene)
│   ├── hook.html           # Scene 1: Kinetic intro (0-8s)
│   ├── early.html          # Scene 2: 2006-2008 early years (8-26s)
│   ├── recovery.html       # Scene 3: 2009-2014 comeback (26-38s)
│   ├── compound.html       # Scene 4: Compounding power (38-53s)
│   ├── structure.html      # Scene 5: Summary + lifestyle (53-67s)
│   ├── reveal.html         # Scene 6: Final amount reveal (67-77s)
│   └── close.html          # Scene 7: CTA closing (77-85s)
│
├── assets/
│   ├── gsap.min.js         # Local GSAP 3.15 (CDN blocked in some envs)
│   ├── sfx/                # 18 sound effect MP3 files
│   │   ├── whoosh.mp3, whoosh-cinematic.mp3
│   │   ├── pop.mp3, ping.mp3, chime.mp3
│   │   ├── impact-bass-1.mp3, impact-bass-2.mp3
│   │   ├── glitch-1.mp3, glitch-2.mp3, glitch-3.mp3
│   │   ├── riser.mp3, sparkle.mp3
│   │   ├── click.mp3, click-soft.mp3, key-press.mp3
│   │   ├── typing.mp3, notification.mp3, error.mp3
│   │   └── whoosh-short.mp3
│   └── voice/
│       └── narration.wav   # TTS-generated voiceover (84.5s)
│
└── renders/                # Output video files
    ├── nifty50-sip-v5.mp4          # Latest landscape (1920x1080)
    ├── nifty50-sip-reels-v3.mp4    # Latest vertical Reels (1080x1920)
    ├── thumbnail-final.jpg          # Custom thumbnail
    ├── cover-hook.jpg, cover-reveal.jpg, cover-summary.jpg
    └── [older versions...]
```

## How to Use / Modify

### Prerequisites

```bash
# Install HyperFrames
npm install -g @anthropic/hyperframes

# For TTS (voice generation)
pip install kokoro-onnx soundfile

# For vertical Reels conversion
# ffmpeg must be installed
```

### Render the Video

```bash
# Set Chrome path if needed
export HYPERFRAMES_BROWSER_PATH="/usr/bin/google-chrome"

# Render landscape version
npx hyperframes render --output renders/output.mp4

# Preview in browser
npx hyperframes preview
```

### Generate Voiceover

```bash
# Generate TTS from narration script
npx hyperframes tts --voice am_adam --speed 1.15 --input narration_script.txt --output assets/voice/narration.wav
```

### Convert to Instagram Reels (Vertical)

```bash
ffmpeg -y \
  -i renders/nifty50-sip-v5.mp4 \
  -filter_complex "
    [0:v]split=2[bg][fg];
    [bg]scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,gblur=sigma=30[blurred];
    [fg]scale=1080:-2[scaled];
    [blurred][scaled]overlay=(W-w)/2:(H-h)/2[base];
    [base]subtitles=captions.srt:force_style='FontName=Arial,FontSize=10,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,BackColour=&H80000000,Bold=1,Outline=2,Shadow=1,MarginV=260,Alignment=2,BorderStyle=4'[out]
  " \
  -map "[out]" -map 0:a \
  -c:v libx264 -crf 18 -preset fast -pix_fmt yuv420p \
  -c:a aac -b:a 192k -r 30 -movflags +faststart \
  renders/reels-output.mp4
```

### Create a Custom Scene

Each scene is a standalone HTML file in `compositions/`. The key pattern:

```html
<template>
<style>
  #root { position: absolute; inset: 0; background: #fbfaf8; }
  /* Your styles */
</style>

<div id="root" data-composition-id="my-scene" data-width="1920" data-height="1080">
  <div id="element" class="clip" data-start="0" data-duration="13">
    <!-- Content -->
  </div>
</div>

<script>
  const tl = gsap.timeline({ paused: true });

  // Animations
  tl.fromTo("#element", {opacity:0}, {opacity:1, duration:0.5}, 0);

  // IMPORTANT: Fade out + hard kill at end
  tl.to("#root", {opacity:0, duration:0.4, ease:"power2.in"}, 12.4);
  tl.set("#root", {opacity:0, visibility:"hidden"}, 12.8);

  window.__timelines["my-scene"] = tl;
</script>
</template>
```

### Key HyperFrames Rules

1. **Clips** need `class="clip"`, `data-start`, `data-duration` (seconds)
2. **Timelines** registered via `window.__timelines["<scene-id>"]`
3. **Hard kills** — every scene MUST have `tl.set("#root", {opacity:0, visibility:"hidden"}, <time>)` after fade-out
4. **Audio** — `<audio>` elements with `data-start`, `data-duration`, `data-volume`, `data-fade-in`, `data-fade-out`
5. **Overlap flags** — use `data-layout-allow-overlap` on clips that intentionally overlap
6. **GSAP** — loaded from local `assets/gsap.min.js` (CDN may be blocked)

## Skills & Tools Used

This project was built using these Claude skills and tools:

| Skill/Tool | Purpose |
|-----------|---------|
| **HyperFrames** | Core HTML-to-video rendering framework |
| **Vox (Design System)** | Color palette, typography, spacing guidelines |
| **Kokoro TTS** | Local text-to-speech for voiceover generation |
| **watch-video** | Video analysis — extracting keyframes for review |
| **BlitzReels** | Instagram Reels optimization toolkit |
| **ffmpeg** | Video conversion, subtitle burning, format conversion |
| **GSAP** | Animation engine for all scene transitions |

## Lifestyle Inflation Data (2006 vs 2025)

Used in the summary scene to show why investing matters:

| Item | 2006 Price | 2025 Price | Multiplier |
|------|-----------|-----------|------------|
| Movie Ticket | ₹50 | ₹350 | 7× |
| Petrol (1L) | ₹47 | ₹106 | 2.3× |
| Gold (10g) | ₹8,600 | ₹95,000 | 11× |
| 1BHK Rent | ₹5,000 | ₹18,000 | 3.6× |
| Thali Meal | ₹30 | ₹120 | 4× |

## Instagram Optimization

See `reels-optimization-guide.md` for:
- Optimized caption text with 30 hashtags
- Best posting times (IST)
- Cover image recommendations
- Engagement tips
- Cross-posting strategy

## License

This project is open source. The SIP data is based on actual Nifty 50 historical returns. The video is for educational purposes only — past performance does not guarantee future returns.

---

Built with [HyperFrames](https://hyperframes.studio) + [Claude](https://claude.ai)
