name: tech-explainer-100s
description: Create a click-through slide explainer — a punchy, visual walkthrough of any technical concept (frameworks, languages, protocols, CS concepts, algorithms, architectures, tools). Use this skill whenever the user asks to explain, visualize, walk through, or understand a technical topic — including phrases like "explain X", "what is X", "how does X work", "show me X", "TL;DR on X", or "X in a nutshell". This is the default response format for explainer requests on technical topics. Do not use for non-technical topics (history, biology, cooking, personal advice), for code-writing tasks where the user wants working code rather than an explanation, or for deep tutorials where the user has explicitly asked for long-form prose.
version: 1.0.0
---
 
# Click-Through Explainer
 
Build a punchy, visual walkthrough of a technical concept as a single HTML artifact with JS-driven slide navigation. Think "X in 100 Seconds" YouTube energy, but click-through and rendered inside what looks like a developer's editor.
 
## When to invoke
 
Trigger on any technical "explain it to me" request. The user wants to *understand* something, not get a tutorial or working code. 
 
If the topic is borderline-technical (e.g., "explain compound interest"), still use the skill — the format works for any conceptual walkthrough. 
 
Do not ask the user clarifying questions before building. Pick a reasonable angle and ship it; iteration is cheap.
 
## Output format
 
A single self-contained HTML artifact saved to `/mnt/user-data/outputs/<topic>-explainer.html`, then presented via `present_files`. The artifact must:
 
- Be one file. All CSS and JS inline. No external runtime dependencies — with one allowed exception, the optional `.pptx` export, see below.
- Have **5–7 slides** navigated with: left/right arrow buttons, keyboard arrow keys, and a clickable progress dot bar.
- Have a **toggleable transcript panel** that slides in from the right, showing the dense narration for the current slide. Button label uses an inline SVG line icon (see Iconography), not emoji.
- Render full-bleed in the artifact viewport. Slides should fill the available space.
- Be styled with the **Atom One Dark theme** (see Visual Style below).
## The arc
 
Every explainer follows this 5–7 slide arc. The beats are non-negotiable; the *content* of each beat adapts to the topic.
 
1. **Hook** — A one-liner saying what it is and why anyone should care. Big bold title, optional tagline. *Example: "Python. The most popular programming language in the world."*
2. **Origin** — When was it invented, by whom, what problem were they solving? If the topic has no clear origin (e.g., a CS concept like recursion), pivot to **categorization**: "It's a [type of thing], in the family of [adjacent things]." *Example: "It's a programming language, in the family of Java, C++, and Ruby."*
3. **The mental model** — The one analogy or visual that makes it click. The "aha" beat. Use the canvas — diagram, metaphor, or simple visual that defines the core idea in plain language. One sentence, max two.
4. **The mechanic** (split across 2-3 slides) — How it actually works. A worked example, a walkthrough, a sequence. **Show, don't tell.** If the topic involves code, a tiny snippet lives *here*, embedded in the mechanic — not as a separate "code slide."
5. **Why it matters** *(optional)* — Real-world payoff. Where it shows up. What it enables. *Example: "Every photo on your phone is a pile of these."* Skip if slides 3–4 already landed this.
6. **Punchline outro** — A clever closer. A callback, a pun, a one-liner. *Example: "And that's [topic]."* **Not** a verdict-grid or pros-vs-cons table — too analytical for the format.

## Information density
 
A click-through is brisk. The genre crams **a lot** of facts. The *visual* on each slide carries the big idea; the *transcript panel* carries the details.
 
Every slide should let the viewer learn **2–3 distinct facts**, not just one. Examples:
- A year + an inventor + the problem they were solving (three facts in the Origin slide)
- A diagram + a label callout + a comparison ("unlike X, this Y")
- A code snippet + the line that does the magic + what that line returns
The transcript is where you dump the trivia, the asides, the "interestingly…" notes that don't fit on the slide. Aim for 2–4 sentences per slide in the transcript.
 
## Tone
 
Witty but neutral. Punchy sentences. Confident verbs.
 
**Avoid:**
- Aggressive snark or "congratulations, you just reinvented X" energy — keep it kind
- Hedging filler ("it's worth noting that…", "generally speaking…")
- Marketing fluff ("revolutionary", "game-changing")
- Lecture voice ("As we can see, the algorithm…")
**Reach for:**
- Strong opening clauses ("Born in 1991." "Two functions. One call.")
- Concrete nouns over abstract ones
- The occasional well-placed pun in the outro

## Visual style — Atom One Dark
 
This is the visual identity. Every explainer should feel like a polished clip rendered inside a developer's actual editor.
 
**Backgrounds:**
- Base: `#282c34` (the main slide background)
- Deep panels (controls, footers, transcript drawer): `#21252b`
- Subtle elevated surfaces (cards, snippet backgrounds): `#2c313a` or `#323842`
- Never use pure black `#000` — it breaks the theme
**Foreground text:**
- Body: `#abb2bf`
- Headlines / near-white: `#dcdfe4`
- Secondary / comments / muted: `#5c6370`
**Accent palette — use these exact hex values, matched to their semantic role:**
- `#e06c75` red — deprecation, errors, "danger" framings
- `#d19a66` orange — numbers, constants
- `#e5c07b` yellow — classes, types, named things
- `#98c379` green — strings, success states, "good" framings
- `#56b6c2` cyan — operators, regex, supporting
- `#61afef` blue — functions, primary accent for most explainers
- `#c678dd` purple — keywords, "magic" framings
When in doubt, blue (`#61afef`) is the default primary accent.
**Typography:**
- Use a monospace font for code, numbers, and "editor" feel: `'JetBrains Mono', 'Fira Code', 'SF Mono', Menlo, Consolas, monospace`
- For body and headlines, use a clean sans-serif: `-apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', sans-serif`
- Headlines should be **large** — 3rem to 6rem for the Hook slide title. Don't be timid.
**Code snippets:**
- Inline `<pre><code>` blocks with manual syntax highlighting using `<span style="color: #c678dd">def</span>` etc. (no external libraries)
- Background `#21252b`, rounded corners, generous padding
- Apply the accent palette to tokens by their role (keywords purple, strings green, functions blue, etc.)

## Iconography — line icons, not emoji
 
Never use emoji (📝, ✕, →, ⚠️, etc.) in the UI chrome. They render inconsistently across OSes and clash with the developer-editor aesthetic. Use inline SVG line icons instead.
 
Define icons once as a hidden SVG sprite at the top of `<body>`, then reuse via `<use href="#icon-id">`:
 
```html
<svg width="0" height="0" style="position:absolute;">
  <defs>
    <symbol id="i-doc" viewBox="0 0 24 24"><path d="M14 3H6a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"/><polyline points="14 3 14 9 20 9"/></symbol>
    <symbol id="i-x" viewBox="0 0 24 24"><line x1="6" y1="6" x2="18" y2="18"/><line x1="6" y1="18" x2="18" y2="6"/></symbol>
    <symbol id="i-arrow-left" viewBox="0 0 24 24"><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></symbol>
    <symbol id="i-arrow-right" viewBox="0 0 24 24"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></symbol>
    <symbol id="i-download" viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></symbol>
    <symbol id="i-alert" viewBox="0 0 24 24"><path d="M10.29 3.86 1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></symbol>
  </defs>
</svg>
```
 
Style all icons with a single class so stroke width stays consistent:
 
```css
.icon { width: 16px; height: 16px; stroke: currentColor; stroke-width: 1.8; fill: none; stroke-linecap: round; stroke-linejoin: round; vertical-align: -3px; }
```
 
Use as `<svg class="icon"><use href="#i-doc"/></svg>` — the icon picks up its parent's color via `currentColor`, so it works in buttons, headers, anywhere.
 
## Layout patterns
 
Each slide is its own layout — don't force one template across all slides. The variety is part of the punch.
 
Suggested patterns by beat:
- **Hook**: Centered giant title, small tagline below, blinking cursor (`▮`) optional
- **Origin**: The verified big element — year, decade/era, or one big word — displayed at 150–200px+, with the rest of the facts arranged around it like satellites. (Don't hard-code a 4-digit year into the layout; a decade or word should look just as deliberate.)
- **Mental model**: Two-column — diagram/illustration on one side, one-sentence framing on the other
- **Mechanic**: Code snippet + annotated callouts pointing to specific lines, OR a step-by-step sequence with arrows
- **Why it matters**: A grid of mini-examples or a single dramatic "X is everywhere" visual
- **Outro**: Large title-card "and that's [topic]." with a tiny callback to the hook

## CSS gotchas — read these before you ship
 
These bugs are easy to introduce and look weird in subtle ways. Bake the fixes into the scaffold from the start.
 
### 1. Slide-stacking specificity (the "ghost slide" bug)
 
If `.slide { display: none }` and you later add slide-type rules like `.origin { display: grid }` to set per-slide layouts, the slide-type rule has equal specificity and comes later — so it wins, and that slide is **never hidden**. Multiple slides render on top of each other and you see ghosted text bleeding through.
 
**Fix:** force the hide/show with `!important`, and use compound selectors for any per-slide layout that needs a different `display` value:
 
```css
.slide { display: none !important; position: absolute; inset: 0; }
.slide.active { display: flex !important; }
.slide.origin.active { display: grid !important; grid-template-columns: auto 1fr; }
.slide.model.active  { display: grid !important; grid-template-columns: 1.1fr 1fr; }
```
 
### 2. The controls bar overlap
 
Fixing `.controls` to `bottom: 0` while slides are `100vh` means the bar covers the bottom of every slide. Grid-heavy slides (4-card layouts, etc.) look truncated.
 
**Fix:** wrap slides in a `.stage` container sized to `calc(100vh - 64px)`, and let the controls bar take the remaining 64px in normal flow:
 
```css
.stage    { position: relative; height: calc(100vh - 64px); overflow: hidden; }
.controls { height: 64px; display: flex; align-items: center; gap: 1rem; }
.slide    { position: absolute; inset: 0; padding: 3rem 4rem; }
```
 
## Implementation template
 
Use this scaffold as the starting point. It has the icon sprite, the stacking-specificity fix, the stage/controls split, and the optional `.pptx` export hook all wired up.
 
```html
<!DOCTYPE html>
<html>
<head><style>
  :root {
    --bg: #282c34; --bg-deep: #21252b; --bg-card: #2c313a; --bg-elev: #323842;
    --fg: #abb2bf; --fg-bright: #dcdfe4; --fg-muted: #5c6370;
    --red: #e06c75; --orange: #d19a66; --yellow: #e5c07b;
    --green: #98c379; --cyan: #56b6c2; --blue: #61afef; --purple: #c678dd;
  }
  * { box-sizing: border-box; }
  html, body { margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--fg); font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', sans-serif; height: 100vh; overflow: hidden; }
  .icon { width: 16px; height: 16px; stroke: currentColor; stroke-width: 1.8; fill: none; stroke-linecap: round; stroke-linejoin: round; vertical-align: -3px; }
 
  .stage { position: relative; height: calc(100vh - 64px); overflow: hidden; }
  .slide { display: none !important; position: absolute; inset: 0; padding: 3rem 4rem; flex-direction: column; animation: fade 0.35s ease; }
  .slide.active { display: flex !important; }
  /* Per-slide grid layouts — must be compound selectors with .active */
  .slide.origin.active { display: grid !important; grid-template-columns: auto 1fr; gap: 4rem; align-items: center; }
  .slide.model.active  { display: grid !important; grid-template-columns: 1.1fr 1fr; gap: 3rem; align-items: center; }
  @keyframes fade { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: none; } }
 
  .controls { height: 64px; background: var(--bg-deep); border-top: 1px solid var(--bg-elev); padding: 0 1.5rem; display: flex; align-items: center; gap: 1rem; }
  .controls button { background: var(--bg-card); border: 1px solid var(--bg-elev); color: var(--fg-bright); font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; padding: 0.45rem 0.85rem; border-radius: 6px; cursor: pointer; display: inline-flex; align-items: center; gap: 0.45rem; }
  .controls button:hover { background: var(--bg-elev); border-color: var(--blue); color: var(--blue); }
  .controls button:disabled { opacity: 0.6; cursor: wait; }
  .controls button .spin { animation: spin 0.9s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .dots { display: flex; gap: 0.55rem; flex: 1; justify-content: center; }
  .dot  { width: 9px; height: 9px; border-radius: 50%; background: var(--fg-muted); cursor: pointer; transition: transform 0.2s; }
  .dot.active { background: var(--blue); transform: scale(1.3); }
 
  .transcript { position: fixed; top: 0; right: -440px; width: 420px; height: 100vh; background: var(--bg-deep); border-left: 1px solid var(--bg-elev); padding: 2.5rem 2rem; transition: right 0.3s ease; overflow-y: auto; z-index: 20; }
  .transcript.open { right: 0; }
</style></head>
<body>
 
<!-- Icon sprite (define once, reuse via <use>) -->
<svg width="0" height="0" style="position:absolute;">
  <defs>
    <symbol id="i-doc" viewBox="0 0 24 24"><path d="M14 3H6a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"/><polyline points="14 3 14 9 20 9"/></symbol>
    <symbol id="i-x" viewBox="0 0 24 24"><line x1="6" y1="6" x2="18" y2="18"/><line x1="6" y1="18" x2="18" y2="6"/></symbol>
    <symbol id="i-arrow-left"  viewBox="0 0 24 24"><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></symbol>
    <symbol id="i-arrow-right" viewBox="0 0 24 24"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></symbol>
    <symbol id="i-download"    viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></symbol>
    <symbol id="i-spinner"     viewBox="0 0 24 24"><line x1="12" y1="2" x2="12" y2="6"/><line x1="12" y1="18" x2="12" y2="22"/><line x1="4.93" y1="4.93" x2="7.76" y2="7.76"/><line x1="16.24" y1="16.24" x2="19.07" y2="19.07"/><line x1="2" y1="12" x2="6" y2="12"/><line x1="18" y1="12" x2="22" y2="12"/><line x1="4.93" y1="19.07" x2="7.76" y2="16.24"/><line x1="16.24" y1="7.76" x2="19.07" y2="4.93"/></symbol>
  </defs>
</svg>
 
<div class="stage">
  <div class="slide hook active" data-transcript="Detailed narration for slide 1...">
    <!-- Hook content -->
  </div>
  <div class="slide origin" data-transcript="Detailed narration for slide 2...">
    <!-- Origin content -->
  </div>
  <!-- ... more slides ... -->
</div>
 
<div class="controls">
  <button onclick="prev()"><svg class="icon"><use href="#i-arrow-left"/></svg> prev</button>
  <div class="dots" id="dots"></div>
  <button onclick="next()">next <svg class="icon"><use href="#i-arrow-right"/></svg></button>
  <button onclick="toggleTranscript()"><svg class="icon"><use href="#i-doc"/></svg> transcript</button>
  <button onclick="downloadPptx(this)"><svg class="icon"><use href="#i-download"/></svg> .pptx</button>
</div>
 
<div class="transcript" id="transcript">
  <button onclick="toggleTranscript()" aria-label="Close" style="position:absolute;top:1rem;right:1rem;background:none;border:none;color:var(--fg-muted);cursor:pointer;">
    <svg class="icon"><use href="#i-x"/></svg>
  </button>
  <div id="t-body"></div>
</div>
 
<script>
  let current = 0;
  const slides = document.querySelectorAll('.slide');
  const dotsContainer = document.getElementById('dots');
  const tBody = document.getElementById('t-body');
  slides.forEach((_, i) => {
    const d = document.createElement('div');
    d.className = 'dot' + (i === 0 ? ' active' : '');
    d.onclick = () => goTo(i);
    dotsContainer.appendChild(d);
  });
  function goTo(i) {
    slides[current].classList.remove('active');
    document.querySelectorAll('.dot')[current].classList.remove('active');
    current = (i + slides.length) % slides.length;
    slides[current].classList.add('active');
    document.querySelectorAll('.dot')[current].classList.add('active');
    tBody.textContent = slides[current].dataset.transcript;
  }
  function next() { goTo(current + 1); }
  function prev() { goTo(current - 1); }
  function toggleTranscript() { document.getElementById('transcript').classList.toggle('open'); }
  document.addEventListener('keydown', e => {
    if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
    if (e.key === 'ArrowRight') next();
    if (e.key === 'ArrowLeft') prev();
    if (e.key === 't' || e.key === 'T') toggleTranscript();
  });
  tBody.textContent = slides[0].dataset.transcript;
 
  /* See ".pptx export" section in SKILL.md for the downloadPptx implementation. */
</script>
</body>
</html>
```
 
Use this as scaffolding — enrich it freely with animations, SVG diagrams, transitions, etc. Keep it one self-contained file.
 
## .pptx export
 
Add a "Download .pptx" button in the controls bar (with the `i-download` icon). Clicking it should produce a 16:9 PowerPoint file where each slide's `data-transcript` becomes the speaker notes.
 
**The one external-dependency exception.** PptxGenJS must be loaded from a CDN at click time. Lazy-load it on first click so the page stays snappy if the user never downloads:
 
```js
let pptxLib = null;
function loadPptxGen() {
  if (pptxLib) return Promise.resolve(pptxLib);
  if (window.PptxGenJS) { pptxLib = window.PptxGenJS; return Promise.resolve(pptxLib); }
  return new Promise((resolve, reject) => {
    const s = document.createElement('script');
    s.src = 'https://cdn.jsdelivr.net/npm/pptxgenjs@3.12.0/dist/pptxgen.bundle.js';
    s.onload  = () => { pptxLib = window.PptxGenJS; resolve(pptxLib); };
    s.onerror = () => reject(new Error('Failed to load PptxGenJS — check your internet connection.'));
    document.head.appendChild(s);
  });
}
```
 
**The pattern for `downloadPptx`:**
 
```js
async function downloadPptx(btn) {
  const original = btn.innerHTML;
  btn.disabled = true;
  btn.innerHTML = '<svg class="icon spin"><use href="#i-spinner"/></svg> building…';
  try {
    const PptxGen = await loadPptxGen();
    const pres = new PptxGen();
    pres.layout = 'LAYOUT_WIDE';       // 13.333 × 7.5 inches = 16:9
    pres.title = '<topic>';
 
    const W = 13.333, H = 7.5;
    const addSlide = (transcript) => {
      const s = pres.addSlide();
      s.background = { color: '282C34' };   // hex with NO leading #
      if (transcript) s.addNotes(transcript);
      return s;
    };
 
    // Build each slide as native PowerPoint shapes/text — do NOT screenshot.
    // For each HTML slide, hand-translate the layout using:
    //   s.addText(string | richTextArray, { x, y, w, h, fontFace, fontSize, color, ... })
    //   s.addShape(pres.ShapeType.ellipse | rect | roundRect | line, { x, y, w, h, fill, line })
    // Coordinates are in INCHES. Colors are hex strings without '#'.
 
    await pres.writeFile({ fileName: '<topic>.pptx' });
  } catch (err) {
    console.error(err);
    alert('Sorry — could not generate the .pptx. ' + (err.message || ''));
  } finally {
    btn.disabled = false;
    btn.innerHTML = original;
  }
}
```
 
**Key requirements for the export:**
 
- **16:9 widescreen** via `pres.layout = 'LAYOUT_WIDE'` (13.333 × 7.5 inches). Do NOT use `'LAYOUT_16x9'` — that's a 10 × 5.625 in mistake on some versions; `LAYOUT_WIDE` is the safe choice.
- **Transcripts → speaker notes** via `slide.addNotes(transcript)`. Pull from each `.slide`'s `data-transcript` attribute so it stays in sync with the on-screen panel.
- **Native shapes, not rasterized images.** The deck should be editable in PowerPoint, with selectable text. Rebuild each HTML slide as PptxGenJS text and shape calls. Yes, this is verbose — write a helper per repeated diagram (e.g. `drawTopologyDiagram(pres, slide, ...)`).
- **Atom One Dark palette, hex without `#`.** PptxGenJS hex strings are 6 chars, no leading `#`. Map your CSS vars once at the top of the function: `const C = { bg: '282C34', blue: '61AFEF', ... }`.
- **Lazy-load on click, never on page load.** Page must still work without internet; export just won't.
**ShapeType gotcha.** `ShapeType` is a property of the PptxGenJS **instance** (`pres`), not the constructor (`PptxGenJS`). When passing a helper around, pass `pres`, not the library reference:
 
```js
// WRONG — pres_.ShapeType is undefined
function drawDiagram(slide, kind) {
  slide.addShape(pptxLib.ShapeType.ellipse, ...);  // ← fails: ShapeType not on constructor
}
 
// RIGHT — pass the instance in
function drawDiagram(pres, slide, kind) {
  slide.addShape(pres.ShapeType.ellipse, ...);
}
```
 
**Per-slide build pattern.** Rebuild each of the 5–7 slides explicitly. Don't try to be clever and reuse a single function — slide layouts vary on purpose. A reasonable per-slide block looks like:
 
```js
{
  const s = addSlide(t(0));    // t(i) returns slides[i].dataset.transcript
  s.addText('// CLICK-THROUGH', {
    x: 0, y: 2.6, w: W, h: 0.4, align: 'center',
    fontFace: 'Consolas', fontSize: 11, color: C.fgMuted, charSpacing: 4
  });
  s.addText([
    { text: 'topic ', options: { color: C.fgBright } },
    { text: 'name',   options: { color: C.blue } }
  ], { x: 0, y: 3.0, w: W, h: 1.6, align: 'center', fontFace: 'Calibri', fontSize: 72, bold: true });
}
```
 
Font choices for the .pptx: use `'Consolas'` for mono (broadest cross-platform support — JetBrains Mono won't ship with PowerPoint), and `'Calibri'` for sans (PowerPoint default).
 
## Research phase — do this before drafting slides
 
Every explainer must rest on **verified facts**, not vibes. The format is punchy and confident, which makes hallucinations especially dangerous: a wrong year or a misattributed inventor reads as authoritative and gets repeated. Front-load the research so the slide content is correct before you start designing.
 
### 1. Deep research
 
Before drafting beats, gather the facts that will populate the slides:
 
- **Origin facts** — invention year, inventor(s), originating institution/company, the problem they were solving. Pull these from primary or near-primary sources (official docs, the project's own history page, Wikipedia's references, papers, RFCs).
- **Mechanic facts** — how it actually works, the canonical worked example, any code snippet (must be syntactically valid and runnable as written).
- **"Why it matters" facts** — concrete deployments, adoption numbers (with year), notable users. Numbers without a year are red flags.
- **Trivia for the transcript** — asides, naming origin, lesser-known anecdotes.
 
Use `WebSearch` and `WebFetch` for anything you are not certain about. Prefer **at least two independent sources** for any specific number, date, or attribution. If you cannot verify a fact, drop it — do not paper over uncertainty with confident phrasing.
 
Capture findings in a brief internal fact sheet before drafting slides:
 
```
- Year invented: <year>            [source: <url or doc>]
- Inventor(s): <names>             [source: <url or doc>]
- Original problem: <one line>     [source: <url or doc>]
- Canonical mechanic example: ...  [source: <url or doc>]
- Adoption / deployment: ...       [source: <url or doc>, year: <year>]
```
 
### 2. Consolidate
 
Once the fact sheet is solid, *then* compress it into the 5–7 slide beats. Each beat's 2–3 facts must trace back to a line in the fact sheet. If a slide needs a fact you don't have, go back to research — don't invent.
 
### 3. Hallucination check (do not skip)
 
Before writing HTML, run an explicit verification pass against every claim that will appear on a slide or in a transcript:
 
- [ ] **Dates and numbers** — every year, version number, percentage, and statistic is sourced. Re-check by searching the specific claim verbatim.
- [ ] **Names and attributions** — every person, company, or institution credited is correctly spelled and correctly attributed. Common pitfalls: co-inventors omitted, the wrong company credited, inventors confused with later popularizers.
- [ ] **Code snippets** — syntax is valid for the named language/version. The snippet does what the slide claims it does. If it uses a specific API, confirm that API exists and behaves as shown.
- [ ] **Causal claims** — "X was created because Y" needs a source. Origin stories are routinely mythologized; verify against the inventor's own account where possible.
- [ ] **Superlatives** — "the first", "the most popular", "the fastest" need a sourced, dated qualifier. If you can't qualify it, soften it or drop it.
- [ ] **No fabricated specifics** — if you wrote a name, date, or number and can't immediately point to where you got it, treat it as suspect and re-verify or remove.
 
If a fact fails the check, fix it before moving on. **Confident-but-wrong is worse than vague-but-honest in this format**, because the punchy tone amplifies the error.
 
## Workflow
 
1. **Research** the topic per the Research phase above. Build the fact sheet. Run the hallucination check. Do not proceed until every slide-bound fact is sourced.
2. Pick the topic's angle: what's the *one thing* you want the viewer to walk away knowing? That goes in the Hook and gets called back in the Outro.
3. Draft the 5–7 slide beats as a bullet list before writing any HTML. Confirm each slide carries 2–3 facts, all traceable to the fact sheet.
4. Write the transcript for each slide in parallel — it's what gives the slide depth when the viewer wants more, and it's what becomes speaker notes in the .pptx. Transcripts get the same hallucination check as slide content.
5. Build the HTML from the scaffold above. Inline everything except the lazy-loaded PptxGenJS. Apply the Atom One Dark palette religiously.
6. Hand-translate each slide to PptxGenJS in `downloadPptx`. Verbose is fine.
7. Final pass: re-read every slide and transcript end-to-end and challenge each specific claim one more time. Anything you can't defend, cut.
8. Save to `/mnt/user-data/outputs/<topic>-explainer.html` and call `present_files`.
Don't narrate the build to the user — they want the artifact, not the play-by-play. A short framing line ("Here's a click-through on [topic] — arrow keys to navigate, `t` to toggle the transcript, `.pptx` button to download.") is plenty.