# Skills & Tools for Animation and Instagram/YouTube Growth

Everything we use to create high-performing finance explainer videos — from animation frameworks to growth strategies.

---

## 1. Animation & Video Production

### HyperFrames (Primary Framework)
- **What**: HTML-to-video renderer. Write HTML/CSS/JS scenes, animate with GSAP timelines, render frame-by-frame to MP4.
- **Install**: `npx hyperframes init <project-dir>`
- **Render**: `export HYPERFRAMES_BROWSER_PATH="/usr/bin/google-chrome" && npx hyperframes render --output renders/output.mp4`
- **Check**: `npx hyperframes check --json` — catches timing errors, overlap issues, GSAP misuse before render.
- **Key rules**:
  - Root `index.html` declares total `data-duration`, mounts scenes from `compositions/`
  - Every visible element needs `class="clip"`, `data-start`, `data-duration`
  - Register GSAP timelines: `window.__timelines["scene-id"] = tl`
  - Never use CSS `transform` on GSAP-animated elements
  - Use `data-layout-allow-overlap` for intentional overlaps
  - Hard-kill scenes after fade-out: `tl.set("#root", {opacity:0, visibility:"hidden"}, endTime)`
  - 1920x1080 at 30fps default

### GSAP (Animation Engine)
- Powers all motion — fades, slides, scale, rotations, counter animations, bar charts
- Loaded locally from `assets/gsap.min.js` (CDN blocked in render environment)
- Key patterns:
  ```js
  // Counter animation
  tl.to(counter, { innerText: targetValue, snap: { innerText: 1 }, duration: 2 }, startTime);
  
  // Staggered entrance
  tl.from(".items", { y: 30, opacity: 0, stagger: 0.15 }, startTime);
  
  // Fade out + hard kill
  tl.to("#scene-inner", { opacity: 0, duration: 0.5 }, fadeTime);
  tl.set("#root", { opacity: 0, visibility: "hidden" }, killTime);
  ```

### Kokoro TTS (AI Voiceover)
- **Generate**: `npx hyperframes tts --input narration_script.txt --output assets/voiceover.wav --voice am_adam --speed 1.15`
- Voice `am_adam` — clear, energetic male narrator
- Speed 1.15x gives punchy, Reels-friendly pacing
- **Per-paragraph timing technique** — generate TTS for each paragraph separately, measure with `ffprobe`, get precise scene start/end times:
  ```bash
  echo "Paragraph text" > /tmp/p1.txt
  npx hyperframes tts --input /tmp/p1.txt --output /tmp/p1.wav --voice am_adam --speed 1.15
  ffprobe -v error -show_entries format=duration -of csv=p=0 /tmp/p1.wav
  ```

### FFmpeg (Video Processing)
- **Vertical conversion** (1080x1920 for Reels, no subtitles):
  ```bash
  ffmpeg -y -i input.mp4 \
    -filter_complex "
      [0:v]split=2[bg][fg];
      [bg]scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,boxblur=25:5[blurred];
      [fg]scale=1080:-2:force_original_aspect_ratio=decrease[scaled];
      [blurred][scaled]overlay=(W-w)/2:(H-h)/2[outv]
    " \
    -map "[outv]" -map 0:a \
    -c:v libx264 -crf 23 -preset medium -pix_fmt yuv420p \
    -c:a aac -b:a 128k -movflags +faststart \
    output-vertical.mp4
  ```
- Creates blurred background + centered landscape video = clean vertical format
- `-movflags +faststart` for instant playback on mobile

---

## 2. Visual Design System (Vox Palette)

Finance explainers use a consistent Vox-inspired design language:

| Token | Hex | Use |
|-------|-----|-----|
| Ink | `#1A1A1A` | Background, dark panels |
| Yellow | `#FFE619` | Highlights, key numbers, accents |
| Process Blue | `#66CFFF` | Secondary data, positive metrics |
| Specimen Blue | `#58BCEC` | Charts, links |
| Light Grey | `#F2F2F2` | Card backgrounds |
| Mid Grey | `#E9E9EB` | Borders, dividers |
| Dark Grey | `#6b7075` | Secondary text |
| Muted Grey | `#b0b5ba` | Labels, annotations |
| White Ground | `#fbfaf8` | Main background |

### Fonts
| Font | Role |
|------|------|
| **Archivo Black** | Headlines, big numbers |
| **Inter** | Body text, labels |
| **Space Mono** | Data, percentages, ticker symbols |
| **EB Garamond** | Serif accents, quotes |

---

## 3. SVG Character Design

For story-driven videos (like the Jensen Huang scandal):

- **Inline SVG characters** with distinct features (leather jacket, hairstyle, accessories)
- **Face pattern**: Draw a skin-tone ellipse as the face opening AFTER the hair path, so skin shows through
- **Character animation**: Use GSAP to animate character entrance, reactions, and exits
- Keep characters simple (flat color, minimal detail) — they're visual anchors, not the focus

---

## 4. Narration & Scripting

### Storytelling Formula for Reels
1. **Hook (0-3s)**: Open with a shocking statement or question — "Imagine this. You build a $3.4 trillion empire..."
2. **Setup (3-10s)**: Give just enough context to create curiosity
3. **Breakdown (10-60s)**: Deliver the meat — use numbered lists, dramatic pauses, and "here's the kicker" moments
4. **Data/Impact (60-75s)**: Show the numbers — charts, percentages, comparisons
5. **CTA (75-90s)**: "Follow for more. Drop a comment — [question]"

### Catchy Narration Techniques
- **Conversational hooks**: "Okay, imagine this", "Let me break it down", "Here's the kicker", "Let that sink in"
- **Direct address**: Talk TO the viewer — "your portfolio", "you need to pay attention"
- **Numbered lists in speech**: "Affair number one...", "Number two..."
- **Scale comparisons**: "That's bigger than Australia's entire GDP"
- **Rhetorical questions**: "Can anyone replace the man in the leather jacket?"
- **Bull/bear framing**: Present both sides so viewer feels informed, not sold to

### Script Length Guide
| Video Length | Word Count | TTS Duration (1.15x) |
|---|---|---|
| 60s Reel | ~250 words | ~55-60s |
| 90s Reel | ~350-400 words | ~80-90s |
| 3min YouTube | ~700 words | ~170s |

---

## 5. Scene Architecture

### Standard 7-Scene Structure (90s Finance Explainer)
| Scene | Duration | Purpose |
|-------|----------|---------|
| Hook | 10-12s | Shocking opener, grab attention in first 3s |
| Story/Scandal | 25-28s | The narrative — what happened and why it matters |
| Impact/Crash | 12-14s | Data visualization — charts, crash animations |
| Winners/Losers | 10-12s | Who gained, who lost — bar charts, comparisons |
| What's Next | 12-14s | Analyst predictions, price targets |
| Trader View | 12-14s | Bull vs bear case, actionable framing |
| CTA | 4-5s | Follow, comment, share prompt |

### Scene Timing Rules
- Each scene gets `data-start` and `data-duration` in seconds
- Add 0.3-0.5s buffer between voiceover end and scene fade-out
- Start fade-out 0.5s before scene end
- Hard-kill (visibility:hidden) 0.1s before scene end
- Audio `data-duration` = actual TTS length (no padding)

---

## 6. Instagram Reels Optimization

### Video Format
- **Landscape**: 1920x1080 (YouTube, LinkedIn)
- **Vertical Reels**: 1080x1920 (Instagram, YouTube Shorts, TikTok)
- No burnt-in subtitles on vertical — Instagram auto-generates captions
- Use `-movflags +faststart` for instant mobile playback

### Hook Strategy (First 3 Seconds)
The algorithm decides in 0.5-1s whether to show your video. Win those seconds:
- **Visual shock**: Big animated number, dramatic character, bold text
- **Audio hook**: Start mid-sentence, not with "Hey guys"
- **Text overlay**: One line that creates an open loop ("He lost $1.6 TRILLION")
- **Motion**: Something must be moving frame 1 — counter, zoom, slide-in

### Posting Strategy
- **Best times** (India): 8-9 AM, 12-1 PM, 7-9 PM IST
- **Hashtags**: Mix of large (#finance, #stockmarket) and niche (#nifty50, #nvidistock)
- **Caption**: Repeat the hook + add a question to drive comments
- **Cover image**: Clean frame from the "reveal" moment with large text

### Engagement Triggers
- Ask a question in the CTA ("Who should be the next CEO?")
- Create debate ("Is this a buying opportunity or a trap?")
- Use "Save this" moments (data tables, key numbers)
- "Share with someone who..." to drive shares
- Post a comment yourself immediately to seed discussion

---

## 7. Content Research Workflow

### For Finance Videos
1. **Web search** current news — Reuters, Bloomberg, CNBC for facts and figures
2. **Stock data** — current price, % changes, market cap, 52-week range
3. **Analyst opinions** — price targets, ratings changes
4. **Dependent companies** — who benefits, who suffers from the news
5. **Historical context** — comparisons to similar events
6. **Cross-verify** every number from at least 2 sources before scripting

### Fact-Checking Rules
- Never invent numbers — every stat must come from a verified source
- Include a "Not financial advice" disclaimer in trader/recommendation scenes
- Date-stamp volatile data ("as of [date]")
- Use "approximately" for rounded figures

---

## 8. Available Claude Skills & Plugins

### Video Production Skills
| Skill | Purpose |
|-------|---------|
| `hyperframes` | Core video framework — compose, animate, render HTML to video |
| `hyperframes-cli` | CLI commands — scaffold, check, preview, render, share |
| `hyperframes-registry` | 450+ ready-made blocks — transitions, charts, captions, effects |
| `media-use` | Music, SFX, voiceover, avatars, footage, color grades |
| `vox-explainer` | 60-90s collage-style explainer template |
| `style-sampling` | Replicate proven video styles on new topics |
| `reverse-any-video` | Rebuild any video as editable HyperFrames project |
| `anthropic-skills:video-making` | General video rendering guidance |

### Editing & Format Skills
| Skill | Purpose |
|-------|---------|
| `blitzreels:blitzreels-editing` | Timeline, caption, media, preview, export editing |
| `blitzreels:blitzreels-clipping` | Create clips from long-form content |
| `blitzreels:blitzreels-generation` | AI-generated media — faceless videos, voiceovers, music, SFX |
| `it-reelsmaker:it-reelsmaker` | Vertical short video editing with subtitles, brand profiles |
| `watch-video:watch-video` | Analyze any video — extract frames + transcript |

### Social & Distribution Skills
| Skill | Purpose |
|-------|---------|
| `blitzreels:blitzreels-x-video` | Turn X/Twitter posts into short-form video |
| `blitzreels:blitzreels-carousels` | Still-slide carousel posts for Instagram |
| `dataviz` | Charts, graphs, dashboards with consistent design system |

### Document & Presentation Skills
| Skill | Purpose |
|-------|---------|
| `anthropic-skills:pptx` | PowerPoint slide decks |
| `anthropic-skills:xlsx` | Excel spreadsheets for data |
| `anthropic-skills:pdf` | PDF creation and manipulation |
| `anthropic-skills:docx` | Word documents |

---

## 9. Project Structure

```
project-name/
├── index.html              # Root composition — declares duration, mounts scenes
├── narration_script.txt    # Voiceover script (plain text, one paragraph per scene)
├── hyperframes.json        # Project config
├── package.json
├── compositions/
│   ├── hook.html           # Scene 1 — attention grabber
│   ├── story.html          # Scene 2 — narrative
│   ├── data.html           # Scene 3 — charts and numbers
│   └── cta.html            # Final scene — call to action
├── assets/
│   ├── gsap.min.js         # Animation engine (local copy)
│   ├── voiceover.wav       # TTS-generated narration
│   └── sfx/                # Sound effects
└── renders/
    ├── video-v1.mp4        # Landscape render
    └── video-v1-vertical.mp4  # Vertical Reels render
```

---

## 10. Quick Reference Commands

```bash
# New project
npx hyperframes init my-video --skip-skills --skill=hyperframes

# Generate voiceover
npx hyperframes tts --input narration_script.txt --output assets/voiceover.wav --voice am_adam --speed 1.15

# Check for errors
npx hyperframes check --json

# Render
export HYPERFRAMES_BROWSER_PATH="/usr/bin/google-chrome"
npx hyperframes render --output renders/output.mp4

# Convert to vertical (Reels/Shorts)
ffmpeg -y -i renders/output.mp4 \
  -filter_complex "[0:v]split=2[bg][fg];[bg]scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,boxblur=25:5[blurred];[fg]scale=1080:-2:force_original_aspect_ratio=decrease[scaled];[blurred][scaled]overlay=(W-w)/2:(H-h)/2[outv]" \
  -map "[outv]" -map 0:a -c:v libx264 -crf 23 -preset medium -pix_fmt yuv420p -c:a aac -b:a 128k -movflags +faststart \
  renders/output-vertical.mp4

# Measure audio duration
ffprobe -v error -show_entries format=duration -of csv=p=0 assets/voiceover.wav
```
