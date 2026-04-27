# Ogilvy Labs Microsite — Claude Code Handoff Prompt

## Before you begin

1. `git checkout main`
2. `git pull origin main`
3. `git checkout -b ogilvy-labs-microsite-v1`

**Confirm with me:** which branch should I be branching from? (Default: `main`.)
**Reminder:** check what branch I'm currently on in Claude Code before running any of the work below.

---

## Context

This repo is a single-file HTML microsite for **Ogilvy Labs** — a public-facing (password-gated, no SEO) site for an experimental product studio inside Ogilvy. It will live on Vercel.

You're starting from a **scaffold file** (`index.html`) that was forked from an earlier internal editorial document. The mechanical work is already done:

- All copy is locked and in place
- Color variable swapped from orange (`#FF6A00`) to Ogilvy red (`#EB3F43`)
- "OC Labs" → "Ogilvy Labs" everywhere
- Meeting-specific sections cut (hero metadata, agenda strip, "What We've Learned," roadmap, hypothesis, discussion)
- Eight new section structure stubbed in
- Agenda JS removed; section ID arrays updated; orphan element references cleaned up

**Your job is the visual rebuild** — the parts that needed to be seen in a browser to get right.

---

## What needs work, in order

### 1. White-to-black hard cut at Section 02

Section 01 (What This Is) sits on the existing white/cream background. From Section 02 (Why This Exists) onward, the page needs to be on black.

The scaffold already applies `section--dark` to Sections 02, 03, 04, 05, 06, 07, 08. The existing `section--dark` styling in the CSS uses `var(--ink)` (`#1A1A1A`) as the background — that should be very close to right, possibly a touch darker (`#0F0F0F` or pure black) depending on how it reads against the film grain.

**Action:**
- Verify the hard cut between Section 01 (white) and Section 02 (dark) reads as intentional and clean — not as a styling bug.
- Tune the dark background tone if needed.
- Verify the film grain (currently 0.035 opacity) reads well on black — it may need to drop to ~0.02 to avoid TV-static feel.
- Verify body text contrast on the dark sections — the existing `.section--dark .prose` rule uses `var(--dim)` (`#999999`) which may be too low contrast for serif body text. Consider `var(--off-white)` (`#F5F0EA`) for body and full white for headings.

### 2. Style the new section stubs

Three sections were stubbed with inline styles as placeholders. Each has a `<!-- TODO (Claude Code): ... -->` comment with direction. They are:

**Section 03 — Three Questions** (`#sec-questions`)
A numbered list of three questions. Should feel similar in weight to the principles list in Section 06, but visually distinct. Treat as a quiet, confident block — large display type for the questions, generous spacing, optional hover/active state.

**Section 04 — What We've Built** (`#sec-built`)
The most visually involved new section. Contains:
- A "BASECAMP" featured block with a heading and one-paragraph description
- Two side-by-side demo cards (TCCC × LA28 Olympics, Sprite × NBA) — should have hover state and clear "walk through the demo →" affordance
- A "From the Room" testimonials block (3 quotes) — match the styling in the screenshot Fran will provide separately: italic Instrument Serif at large size, role/company in JetBrains Mono in red, thin red rule between each
- A secondary row of three smaller cards (Debrief, Promptuary, Network Slicing Arena) — visually subordinate to Basecamp, "Try it →" affordance

**Section 07 — What's Next** (`#sec-next`)
Currently a static `<ul>` of 5 items. Needs to become a quiet rotating ticker — items cycle one at a time, smooth crossfade or vertical scroll, ~4–5 second hold per item. Italic Instrument Serif at large display size. Pause on hover. Should reward attention but not demand it.

**Section 08 — Get in Touch** (`#sec-contact`)
Two contact rows (Frances, Bidnam) with name in display type, email in mono red. Mostly already styled — verify it reads clean on dark.

### 3. Retune the Product Taxonomy interactive (Section 05)

The 3D flask interactive was originally designed for a warm light background. On dark, the existing tier colors will feel muddy.

Tier color palette is defined in two JS objects in the file:
```js
const TIER_COLORS = { 0: '#C8A882', 1: '#CCA06E', 2: '#D4924E', 3: '#DC8236', 4: '#E8721E', 5: '#EB3F43' };
const TIER_BG = { 0: '#F5F0EA', 1: '#EDE7DD', 2: '#E8DFD2', 3: '#E3D7C8', 4: '#DDCFBE', 5: '#F0E4D4' };
const TIER_NUM_COLORS = { 0: '#C8A882', 1: '#BF9A6E', 2: '#B08A55', 3: '#A67A3D', 4: '#D07020', 5: '#EB3F43' };
```

Current palette is a brown-to-red gradient. On dark, this will need to shift — consider a cream-to-red gradient instead so the lower tiers stay visible against the dark backdrop. The Three.js flask material color is also currently red (`0xEB3F43`) — verify it reads at the right intensity against the dark canvas.

Open the page, click through all six tiers, and tune until each one reads with appropriate weight against the dark bg.

### 4. Wire up demo links

The two Basecamp demo cards (`data-demo="tccc-la28"` and `data-demo="sprite-nba"`) currently link to `#`. Bidnam will be building a demo flow with tooltips on a branch of the existing Sprite × NBA Foundry/Podium repo, with a `?demo=true` flag and tooltip overlay system. The two demo URLs will need to be wired in once those branches ship — for now leave as `#` with a TODO.

The three secondary cards (Debrief, Promptuary, Network Slicing Arena) also link to `#` — Fran will provide the live URLs for these.

### 5. Vercel deploy with password protection

Once the page renders cleanly:
1. Connect repo to Vercel
2. Enable password protection on production deployment (Vercel Pro feature)
3. Confirm "no SEO" — add `<meta name="robots" content="noindex,nofollow">` to the `<head>`
4. Test the password gate works
5. Share the production URL + password with Fran

---

## Order of operations

I'd suggest:

1. **First commit:** drop in the scaffold `index.html`, no changes
2. **Second commit:** white-to-black cut + dark section tuning (item 1)
3. **Third commit:** Section 04 styling — the most involved (item 2, focusing on `#sec-built`)
4. **Fourth commit:** Sections 03, 07, 08 styling (rest of item 2)
5. **Fifth commit:** Taxonomy retune (item 3)
6. **Sixth commit:** demo link placeholders + meta tags + Vercel setup (items 4 + 5)

Each commit should be reviewable on its own. After commit 2, deploy a preview to Vercel so Fran can see the dark cut working before you proceed to the more detailed styling work.

---

## Things to NOT touch

- The hero animation (preloader flood + title reveal) — works as-is, just verify the red flood feels intentional rather than alarming
- The CSS variable system — extend it if you need new tokens, but don't refactor the existing ones
- The Lenis smooth scroll setup
- The film grain mechanic (just tune opacity)
- The principles list styling in Section 06 — it's the reference treatment for the new questions list in Section 03

---

## Section map (final)

| # | ID | Section | Background |
|---|---|---|---|
| Hero | — | Ogilvy Labs / "Where Ogilvy's strategic thinking takes more forms." | White |
| 01 | `#sec-what` | What This Is | White |
| 02 | `#sec-why` | Why This Exists | **Dark (hard cut)** |
| 03 | `#sec-questions` | Three Questions | Dark |
| 04 | `#sec-built` | What We've Built (Basecamp + testimonials + secondary tools) | Dark |
| 05 | `#sec-stack` | A Product Taxonomy (interactive flask) | Dark |
| 06 | `#sec-how` | How We Work (4 principles) | Dark |
| 07 | `#sec-next` | What's Next (rotating ticker) | Dark |
| 08 | `#sec-contact` | Get in Touch | Dark |

---

## Open questions to confirm with Fran before final deploy

1. The `frances.hanson@ogilvy.com` and `bidnam.lee@ogilvy.com` email addresses — confirm exact format
2. Demo URLs for Debrief, Promptuary, Network Slicing Arena
3. Password choice for Vercel gate
4. Final repo name (suggested: `ogilvy-labs-site`)
