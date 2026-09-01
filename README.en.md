# PPT Guider

English | [中文](README.md)

> "AI is the co-pilot, not the autopilot."

## Why Guider

Most AI PPT tools promise one-click generation: input a topic, get a "finished" deck. But after trying many, we found that **the best results are never produced by AI alone**. A truly good presentation needs human judgment at key moments: is the outline logical, does the style fit the occasion, is each page too dense or too sparse.

PPT Guider's philosophy: **AI drives the workflow, gathers materials, and generates design drafts. You make the decisions at every checkpoint.** It works like a professional design firm, where you are the creative director. Every step requires your confirmation, and every step can be rolled back.

## What We Tried

### Sticky Note Sidebar Tab

During Step 2 outline review, we built a draggable sticky-note grid view, registered as a sidebar tab via `ctx.betterSidebar.registerTab()`, following the same pattern as [ego-browser](https://github.com/Fisfzy/ego-browser). The basic implementation worked, but the interaction experience (drag-and-drop, click-to-edit) needed more polish in the sidebar sandbox environment. We shelved it for now.

### SVG as the Intermediate State

We tried having AI generate PPTX directly. The results were unpredictable: broken formatting, collapsed layouts. We also tried HTML-to-PPT conversion, which was similarly unstable.

**SVG turned out to be the sweet spot.** It is a format AI excels at generating (text + code), and PowerPoint can import it directly (drag in, then Convert to Shape to edit). More importantly, SVG enables a **two-layer separation**: grayscale planning drafts to lock in structure first, then colored design drafts for visual polish. This prevents the common AI PPT problem where design and content drift apart.

But SVG has a cost: PowerPoint's Convert to Shape cannot faithfully reproduce every SVG feature. We had to impose hard restrictions: no gradients, no filters, no defs, no opacity. **The visual quality suffers, and this is our biggest regret.**

## Why We're Sharing This

This project is far from finished. We had many ideas we couldn't complete or had to abandon:

- **Sticky-note interaction**: visual drag-and-drop outline editing, shelved before the UX was polished
- **Unrestricted SVG conversion**: if we could lift the filter, gradient, and defs restrictions, design quality would jump significantly
- **Native PPT shape generation**: generate PPT native shapes directly, skipping the SVG middle layer entirely. In theory this would be perfect, but the complexity is daunting

We're sharing this to **crowdsource ideas**. If you have a better SVG-to-PPT conversion approach, want to continue the sticky-note interaction design, or have a completely different vision, fork it and let's explore together.

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

When PowerPoint imports SVGs containing Chinese text, the file MUST begin with BOM (EF BB BF). Without it, the entire page fails to render. We learned this the hard way: 1 out of 15 pages failed because of a missing BOM.

### Convert to Shape is Lossy

We disabled `<filter>`, `<defs>`, gradients, `opacity`, and CSS `<style>`, using solid colors and offset dark rectangles to simulate shadows instead. **If you have a better SVG-to-PPT conversion approach, this is where help is most needed.**

## Installation

```bash
git clone https://github.com/chiang21fcb/dsh-ppt-guider.git ~/.dsh/.agent-presets/ppt-guider/
```

Restart DSH, then select "PPT Guider" from the preset list.

## License

MIT