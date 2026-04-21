# font-hunt

> A Claude Code plugin that researches **genuinely unique fonts** from real indie foundries — and refuses to return Inter, Montserrat, Poppins, Roboto, Playfair Display, DM Sans, Satoshi, Clash Display, or any of the other fonts every AI tool keeps serving up.

**Built by [likeahuman.ai](https://likeahuman.ai).**

---

## What it does

Every AI-driven design flow defaults to the same twelve fonts. `font-hunt` actively researches alternatives from real sources the LLM doesn't have strong priors on — Fontshare, Velvetyne, Pangram Pangram, Klim, Grilli Type, OH no Type Co, 1001fonts, DaFont, Google Fonts gems, Adobe Fonts, and more.

Every run produces two artefacts:

1. **A premium HTML specimen board** — editorial layout in the tradition of Klim / OH no Type / Grilli Type specimen sites. Massive type renderings at 180px, generous whitespace, off-white canvas, navy accent, Dutch/European glyph sampler, click-to-copy CSS, `J` / `K` keyboard nav. Saved to `~/.claude/font-hunt-results/`.

2. **A structured YAML brief** printed to chat — font names, foundries, URLs, licensing tier, `@font-face` snippets, pairing rationale. Downstream skills and agents (`typography-creative-director`, `/frontend-design`, `/ai-branding-pitch`) consume this so they don't fall back to training-data defaults.

---

## Install

```
/plugin install likeahuman-ai/font-hunt
```

Or add to your `.mcp.json` / plugin config pointing at this repo.

---

## Usage

```bash
/font-hunt                                        # 4-question micro-interview (free/paid/hybrid + voice)
/font-hunt "editorial wellness brand, warm"       # one-shot brief
/font-hunt --copy "Your real headline"            # override specimen copy
/font-hunt --free-only                            # skip paid-indie researcher
/font-hunt --exclude fontshare,google             # drop specific sources
/font-hunt --out ./custom/path.html               # override output location
```

### First question asked every run

> **Free only, paid indie welcome, or hybrid?**

This gates which researcher tiers run so you don't get recommendations you can't (or won't) use.

---

## How it works

On every invocation:

1. **Brief capture** — args parsed, interview run if missing, mood keywords extracted.
2. **Reference load** — static anti-slop blocklist + recent-picks cache (last 20 picks) + any user-supplied "sick of" fonts are combined into the pass-through blocklist.
3. **Source routing** — brief mood keywords matched against `source-map.md` to decide which agents dispatch.
4. **Parallel dispatch** — up to 5 font-researcher subagents run in parallel, each hunting a different tier of sources with a rotating subset per run.
5. **Synthesis** — candidates flattened, deduped, blocklist-filtered, ranked, assembled into 3 pairings + 2 wildcard heroes with intentional tier-diversity.
6. **Artefacts written** — HTML specimen board + structured YAML block.
7. **Cache updated** — final picks appended to FIFO-capped recent-picks list so the next run rotates.

### The five researcher agents

| Agent | Sources | Voice |
|-------|---------|-------|
| `font-researcher-free-curated` | Fontshare, Pangram Pangram, League of Moveable Type, Fontsource, Uncut.wtf, Open Foundry | Quiet confidence. Craft brands actually ship. |
| `font-researcher-experimental` | Velvetyne, Future Fonts, Use & Modify, Collletttivo | "Too weird" is a compliment. OSS + rule-breaking. |
| `font-researcher-paid-indie` | Klim, Grilli, Dinamo, OH no Type, Sharp, Colophon, Commercial, Production, Displaay | Type director with a real budget. |
| `font-researcher-display-feral` | 1001fonts, DaFont, FontSpace | Hunts the 10% of personality-rich gems among user-submitted noise. |
| `font-researcher-google-adobe` | Google Fonts deep catalogue + Adobe Fonts + Fonts In Use signal | Refuses top-20 defaults at gunpoint. |

---

## Anti-slop guarantees

- **Static blocklist** — 30 fonts refused by every agent AND by the synthesiser (defence in depth). Full list in `skills/font-hunt/references/anti-slop-blocklist.md`.
- **Recent-picks rotation** — last 20 recommendations are added to each agent's blocklist; two back-to-back runs with the same brief produce different fonts.
- **Source rotation per agent** — each agent owns 4–9 sources but hits 2–3 per run; different foundries get sampled on repeat runs.
- **Diversity guard in synthesis** — if the final 5 picks all come from ≤2 sources, the synthesiser force-swaps one pick for a lower-ranked candidate from a different source.
- **Agent voice specialisation** — five distinct hunter personalities physically can't converge on the same fonts.

---

## Example output

Run:
```
/font-hunt "editorial wellness brand, warm, not corporate"
```

Chat prints a pairing table + YAML block. HTML specimen board saved to `~/.claude/font-hunt-results/font-hunt-editorial-wellness-brand-warm-not-corporate.html`.

Example pairings from a real run:

| # | Heading | Body | Tier |
|---|---------|------|------|
| 1 | Heldane Display · Klim | Ohno Softie · OH no Type | paid · paid |
| 2 | Fraunces · Google | Newsreader · Google | free · free |
| 3 | Gambetta · Fontshare | Synonym · Fontshare | free · free |

Plus wildcards: Basteleur (Velvetyne · OSS), GT Sectra (Grilli · paid).

---

## Repository layout

```
font-hunt/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── font-hunt/
│       ├── SKILL.md
│       ├── references/
│       │   ├── anti-slop-blocklist.md
│       │   ├── source-map.md
│       │   └── specimen-template.html
│       ├── scripts/
│       │   └── build-preview.mjs
│       └── assets/
│           └── preview-styles.css
├── agents/
│   ├── font-researcher-free-curated.md
│   ├── font-researcher-experimental.md
│   ├── font-researcher-paid-indie.md
│   ├── font-researcher-display-feral.md
│   └── font-researcher-google-adobe.md
├── README.md
├── CHANGELOG.md
├── LICENSE
└── package.json
```

---

## Philosophy

> "The best font for your brand is the one no one else in your category has thought to use yet."

Typography carries ~80% of a design's personality. Most AI tooling treats font selection as a lookup against a tiny cached list. `font-hunt` treats it as a research task — live-fetching from real foundries, caring about licensing tier, respecting mood, rotating sources, refusing to repeat itself.

If your designer's instinct is "that Montserrat / Poppins / DM Sans thing looks like every other AI output," this plugin is for you.

---

## Contributing

Issues and PRs welcome at [github.com/likeahuman-ai/font-hunt](https://github.com/likeahuman-ai/font-hunt).

Particularly interested in:
- Additional indie foundry sources for the researcher agents
- New anti-slop candidates as fonts drift from "indie gem" into "new default"
- Localisation research (CJK, Arabic, Cyrillic — currently Latin-focused)

---

## Licence

[MIT](./LICENSE) — use freely in personal and commercial work.
