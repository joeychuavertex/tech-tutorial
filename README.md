# tech-explainer-100s

A skill that turns any technical concept into a **click-through slide explainer** — a punchy, visual walkthrough rendered as a single self-contained HTML file. Think "X in 100 Seconds" energy, styled to look like a developer's editor.

## What it does

Ask Claude to explain a technical topic ("explain X", "how does X work", "TL;DR on X") and this skill builds a 5–7 slide deck that:

- Walks through a fixed narrative arc: **Hook → Origin → Mental model → Mechanic → Why it matters → Outro**
- Navigates via arrow keys, on-screen buttons, and a clickable progress-dot bar
- Includes a toggleable **transcript panel** with denser narration for each slide
- Is styled in the **Atom One Dark** theme with monospace/editor aesthetics
- Exports to **16:9 PowerPoint** (`.pptx`), where each slide's transcript becomes speaker notes
- Ships as **one HTML file** with all CSS/JS inline (the only external dependency is PptxGenJS, lazy-loaded on export)

Every explainer is grounded in a **research + hallucination-check pass** before any slide is drafted — dates, names, code, and superlatives are sourced and verified, since the confident tone makes errors especially costly.



## Theme

Every explainer is styled in the **Atom One Dark** palette for a polished, developer-editor feel. The accent colors map to semantic roles:

| Token | Hex | Role |
| --- | --- | --- |
| 🔴 Red | `#e06c75` | Deprecation, errors, "danger" framings |
| 🟠 Orange | `#d19a66` | Numbers, constants |
| 🟡 Yellow | `#e5c07b` | Classes, types, named things |
| 🟢 Green | `#98c379` | Strings, success states, "good" framings |
| 🔵 Cyan | `#56b6c2` | Operators, regex, supporting |
| 🔵 Blue | `#61afef` | Functions, primary accent (default when in doubt) |
| 🟣 Purple | `#c678dd` | Keywords, "magic" framings |

**Surfaces & text:**

| Use | Hex |
| --- | --- |
| Base background | `#282c34` |
| Deep panels (controls, footers, transcript) | `#21252b` |
| Elevated surfaces (cards, snippets) | `#2c313a` / `#323842` |
| Body text | `#abb2bf` |
| Headlines / near-white | `#dcdfe4` |
| Secondary / muted / comments | `#5c6370` |

## When it triggers

- ✅ Explaining technical topics — frameworks, languages, protocols, CS concepts, algorithms, architectures, tools
- ❌ Non-technical topics (history, biology, cooking), code-writing tasks where you want working code, or long-form prose tutorials

## Usage Example

> **You:** explain agent harness

Claude researches the topic, then ships a single self-contained deck. See the included sample:

- [agent-harness-explainer_100.html](agent-harness-explainer_100.html) — a click-through explainer on agent harnesses.

Open it in any browser: arrow keys (or the on-screen buttons) to navigate, `t` to toggle the transcript panel, and the `.pptx` button to export 16:9 slides with the transcript as speaker notes.

## Installation

To use this skill with Claude:

1. Open **Claude → Settings → Customize Claude → Create New Skill**
2. Select **Upload Zip File**
3. Zip this repository and upload it
4. Start asking Claude to generate diagrams with `/tech-explainer-100s`

## License

[MIT](LICENSE)
