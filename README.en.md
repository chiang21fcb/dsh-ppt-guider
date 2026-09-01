# PPT Guider

English | [中文](README.md)

<div align="center"><img src="PPT Guider.png" width="200" alt="PPT Guider"></div>

<div align="center">

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Release](https://img.shields.io/github/v/release/chiang21fcb/dsh-ppt-guider)](https://github.com/chiang21fcb/dsh-ppt-guider/releases)
[![DSH](https://img.shields.io/badge/DSH-Preset-2563eb)](https://github.com/deepseek-ai/deepseek-harness)

</div>

> "AI is the co-pilot, not the autopilot."

PPT Guider is an Agent preset for [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (DSH). It runs inside the DSH environment, driving the full PPT workflow through Agent conversations. You need to install DSH and start the web service before using it.

🚀 [Why Guider](#why-guider) | 🧪 [What I Tried](#what-i-tried) | 💡 [Why I'm Sharing](#why-im-sharing) | 📋 [Workflow](#workflow) | ⚠️ [Known Limitations](#known-limitations) | 📦 [Installation](#installation)

## Why Guider

Most AI PPT tools promise one-click generation: input a topic, get a "finished" deck. But after trying many, I found that **the best results are never produced by AI alone**. A truly good presentation needs human judgment at key moments: is the outline logical, does the style fit the occasion, is each page too dense or too sparse.

PPT Guider's philosophy: **AI drives the workflow, gathers materials, and generates design drafts. You make the decisions at every checkpoint.** It works like a professional design firm, where you are the creative director. Every step requires your confirmation, and every step can be rolled back.

## Quick Start

```bash
# Install the preset
git clone https://github.com/chiang21fcb/dsh-ppt-guider.git ~/.dsh/.agent-presets/ppt-guider/

# Restart DSH, select "PPT Guider" preset
# Then tell the Agent:
#   "Make me a PPT about the AI industry, about 10 pages, business style"
# Or:
#   "Here's our product doc [link], turn it into a PPT"
```

The Agent will follow the six-step workflow, waiting for your confirmation at each stage.

## What I Tried

### Sticky Note Sidebar Tab

During Step 2 outline review, I built a draggable sticky-note grid view, registered as a sidebar tab via `ctx.betterSidebar.registerTab()`, following the same pattern as [ego-browser](https://github.com/Fisfzy/ego-browser). The basic implementation worked, but the interaction experience (drag-and-drop, click-to-edit) needed more polish in the sidebar sandbox environment. I shelved it for now.

### SVG as the Intermediate State

I tried having AI generate PPTX directly. The results were unpredictable: broken formatting, collapsed layouts. I also tried HTML-to-PPT conversion, which was similarly unstable.

**SVG turned out to be the sweet spot.** It is a format AI excels at generating (text + code), and PowerPoint can import it directly (drag in, then Convert to Shape to edit). More importantly, SVG enables a **two-layer separation**: grayscale planning drafts to lock in structure first, then colored design drafts for visual polish. This prevents the common AI PPT problem where design and content drift apart.

But SVG has a cost: PowerPoint's Convert to Shape cannot faithfully reproduce every SVG feature. I had to impose hard restrictions: no gradients, no filters, no defs, no opacity. **The visual quality suffers, and this is my biggest regret.**

## Why I'm Sharing

This project is already working stably and can reliably produce PPTs. But it's not yet at the level I envision. I had many ideas I couldn't complete or had to abandon:

- **Sticky-note interaction**: visual drag-and-drop outline editing, shelved before the UX was polished
- **Unrestricted SVG conversion**: if I could lift the filter, gradient, and defs restrictions, design quality would jump significantly
- **Native PPT shape generation**: generate PPT native shapes directly, skipping the SVG middle layer entirely. In theory this would be perfect, but the complexity is daunting

I'm sharing this to **crowdsource ideas**. If you have a better SVG-to-PPT conversion approach, want to continue the sticky-note interaction design, or have a completely different vision, fork it and let's explore together.

## Models

**DeepSeek Flash** delivers solid quality for daily use. In practice, vision-capable models produce even better results: **K3**, for example, performs particularly well in the planning and design draft stages.

## Workflow

```
Information Gathering → Outline Planning → Supplement Check
  → Planning Draft (grayscale SVG) → Design Draft (color SVG) → Auto-Import to PPTX
```

Every step has a confirmation gate:

| Step | What It Does | Your Role |
|------|-------------|-----------|
| Information Gathering | Reads your materials + web search supplement | Provide topic or materials |
| Outline Planning | Pyramid Principle structured outline | Approve all or review page by page |
| Supplement Check | Anything missing | Add or skip |
| Planning Draft | Grayscale SVG wireframes, lock in layout | Confirm structure |
| Design Draft | Bento Grid color SVG | Review visuals |
| Import to PPTX | Auto-insert into PowerPoint | Manually Convert to Shape |

## Styles

| Style | Background | Primary | Secondary | Accent |
|-------|-----------|---------|-----------|--------|
| 🔵 Tech | #1a1a2e | #16213e | #0f3460 | #e94560 |
| 🟢 Business | #f8f9fa | #1a3a5c | #2c5f8a | #d4a853 |
| 🟡 Academic | #ffffff | #333333 | #2563eb | #1e40af |
| 🟣 Creative | #faf5ff | #7c3aed | #db2777 | #f59e0b |
| ⚪ Minimalist | #ffffff | #111111 | #666666 | #000000 |

## Known Limitations

### BOM Encoding is a Hard Requirement

When PowerPoint imports SVGs containing Chinese text, the file MUST begin with BOM (EF BB BF). Without it, the entire page fails to render. I learned this the hard way: 1 out of 15 pages failed because of a missing BOM.

### Convert to Shape is Lossy

I disabled `<filter>`, `<defs>`, gradients, `opacity`, and CSS `<style>`, using solid colors and offset dark rectangles to simulate shadows instead. **If you have a better SVG-to-PPT conversion approach, this is where help is most needed.**

## Installation

```bash
git clone https://github.com/chiang21fcb/dsh-ppt-guider.git ~/.dsh/.agent-presets/ppt-guider/
```

Restart DSH, then select "PPT Guider" from the preset list.

## License

MIT © chiang21fcb