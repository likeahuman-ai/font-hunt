# Changelog

All notable changes to `font-hunt` will be documented in this file.

## [1.1.1] — 2026-04-21

Bugfix release.

### Fixed
- **Plugin install was failing** with `Validation errors: agents: Invalid input`. The `agents` key is not in the plugin.json schema — agents are auto-discovered from the `agents/` directory. Removed the invalid field.

## [1.1.0] — 2026-04-21

Claude Design compatibility + modern skill schema.

### Added
- **`when_to_use` frontmatter field** following the latest Agent Skills schema — better automatic triggering by Claude, especially for natural-language prompts that don't say the word "font".
- **Claude Design (claude.ai/design) interop section in README.** `font-hunt` is now explicitly documented as a pre-design (feed font choices into Claude Design as context) or implementation-time (lock fonts before building a Claude Design handoff bundle) complement. No native plugin API on claude.ai/design yet — this skill runs in Claude Code which is the surface where handoffs land.

## [1.0.0] — 2026-04-20

Initial release.

### Added
- `/font-hunt` slash command with free/paid/hybrid gating question
- Five parallel font-researcher subagents:
  - `font-researcher-free-curated` (Fontshare, Pangram Pangram, League of Moveable Type, Fontsource, Uncut.wtf, Open Foundry)
  - `font-researcher-experimental` (Velvetyne, Future Fonts, Use & Modify, Collletttivo)
  - `font-researcher-paid-indie` (Klim, Grilli, Dinamo, OH no Type, Sharp, Colophon, Commercial, Production, Displaay)
  - `font-researcher-display-feral` (1001fonts, DaFont, FontSpace)
  - `font-researcher-google-adobe` (Google Fonts gems, Adobe Fonts, Fonts In Use signal)
- Anti-slop blocklist refusing 30 AI-default fonts (Inter, Montserrat, Poppins, Roboto, Playfair Display, DM Sans, Satoshi, Clash Display, General Sans, Geist, etc.)
- Recent-picks cache (last 20 recommendations) — same brief run twice produces different fonts
- Mood-to-source routing in `references/source-map.md`
- Editorial HTML specimen board rendered at `~/.claude/font-hunt-results/` with IntersectionObserver fade-ins, J/K keyboard nav, click-to-copy CSS, Dutch glyph sampler
- Structured YAML chat output consumable by downstream agents/skills
- `build-preview.mjs` Node ESM script with function-replacement pattern (safe against `$&` / `$1` token traps in user briefs)
- `cssFontName` helper stripping quotes from font names before CSS-in-HTML-attr embedding
