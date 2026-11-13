GOAL:    Viewer understands Jensen Huang's resignation scandal, its market impact, and what traders should watch — entertained by the scandal hook, informed by the Vox-style data sections
OUTPUT:  HyperFrames, 1920 x 1080, 30 fps
TIME:    90s × 30fps = 2700 frames
LOOK:    Vox palette — ground #fbfaf8, ink #1A1A1A, yellow #FFE619, blue #58BCEC/#66CFFF, greys #F2F2F2/#E9E9EB/#8d9399
         Display: Archivo Black 72-96px, Body: Inter 28-36px, Data: Space Mono 48-64px
         Scenes 1-2: dramatic dark ground (#1A1A1A) with yellow/red accents for scandal energy
         Scenes 3-7: Vox light ground (#fbfaf8) with ink text, yellow/blue data highlights
OBJECTS: Jensen Huang figure (leather jacket silhouette), stock chart, company logos (AMD/NVDA/BCOM),
         bull/bear icons, analyst price targets as falling numbers
MOTION:  Enter: power3.out 0.6s, Exit: power2.in 0.4s, Data count-up: 1.4s power1.inOut
         Staggers: 0.08s between items, Charts draw over 1.2s
         stepEase at 12fps for character animations
SHOTS:
  0-8.6s    HOOK — dark ground, Jensen silhouette with $3.4T, scandal headline smashes in
  8.6-25.4s SCANDAL — timeline cards: spy affair → VP affair → resignation, board stamp
  25.4-39.6s CRASH — Vox style, NVDA line chart crashing, $1.6T counter, Australia GDP comparison
  39.6-51.5s WINNERS/LOSERS — split screen, AMD/BCOM bars growing green vs CoreWeave/TSMC red
  51.5-71.7s WHAT'S NEXT — analyst price target waterfall, interim CEO card, earnings countdown
  71.7-85.4s TRADER — bull vs bear split, yellow vs blue, key points animate in
  85.4-90s   CTA — follow prompt with social icons
BANNED: black void, glow/bloom, purple gradients, floating particles, glass cards, corner brackets,
        three-cards-in-a-row, everything fading in at once, KPI grids
QA:     Frame 0 (thumbnail test), 4.0s (scandal hit), 15s (timeline mid), 30s (chart crash),
        45s (winners bars), 60s (analyst targets), 75s (bull/bear split), 87s (CTA)
