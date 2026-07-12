# Liftd Design System

**Status:** Approved direction, implementation in progress
**Direction:** Performance Ledger
**Approved prototype:** 11-07-2026

## Design thesis

Liftd is a precise training ledger built for the seconds between heavy sets. Competition-log structure provides speed and alignment; a low-glare performance atmosphere gives it character; compact weekly summaries add context without turning it into a wellness dashboard.

Physical scene: a lifter checks a phone one-handed under hard gym lighting, focused and slightly fatigued. The interface must remain legible at a glance, resist glare, and keep the next action in the thumb zone.

## Product posture

- Serious tool, not motivational coach.
- Personal record, not social performance.
- Dense where comparison matters, quiet everywhere else.
- Familiar mobile controls, no invented gestures or decorative interactions.
- Mobile only. Desktop viewports centre the mobile shell but do not gain a desktop information architecture.

## Reference synthesis

- [Hevy logging](https://mobbin.com/screens/1c4a0238-dffb-4b0e-9dfb-e74d1ef62fb1): adjacent previous/set data and dense completion structure.
- [Tonal progress](https://mobbin.com/screens/49b9079a-87e6-434d-802e-25a9a9858077): record hierarchy and rep-specific performance.
- [Fitbod progress](https://mobbin.com/screens/eaec425d-75fe-4f6e-972e-9ac2ba7700ff): focused dark performance atmosphere and exercise-level progression.
- [Bevel strength overview](https://mobbin.com/screens/602918f6-a810-4f63-beb3-909f431ee787): compact summary density, used sparingly.

We borrow interaction principles, not branded component styling or product scope.

## Colour strategy

Restrained. Green-yellow occupies less than 10% of the interface and is reserved for primary actions, active values, completion, current navigation, selected filters, and positive personal-record emphasis.

```css
:root {
  --color-base: oklch(15% 0.012 145);
  --color-base-soft: oklch(18% 0.014 145);
  --color-surface: oklch(20% 0.014 145);
  --color-surface-active: oklch(24% 0.016 145);
  --color-surface-pressed: oklch(28% 0.018 145);
  --color-line: oklch(31% 0.013 145);
  --color-line-strong: oklch(40% 0.018 145);
  --color-text: oklch(95% 0.008 105);
  --color-text-soft: oklch(72% 0.012 145);
  --color-text-muted: oklch(58% 0.012 145);
  --color-accent: oklch(88% 0.20 125);
  --color-accent-pressed: oklch(81% 0.19 125);
  --color-accent-ink: oklch(21% 0.04 125);
  --color-success: oklch(76% 0.17 145);
  --color-warning: oklch(78% 0.15 78);
  --color-danger: oklch(67% 0.18 25);
  --color-focus: oklch(91% 0.17 125);
}
```

Depth comes from lighter surfaces, not visible shadows. Alpha is limited to focus rings, backdrops, and Chart.js fills.

## Typography

Use one native system sans stack for zero loading delay and platform familiarity:

```css
font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", system-ui, sans-serif;
```

- Brand/page title: 2rem, 720, tight optical tracking.
- Screen title: 1.5rem, 680.
- Section/data heading: 1.0625rem to 1.5rem, 620 to 680.
- Body and input labels: 1rem minimum where sustained reading or entry matters.
- Secondary data: .875rem.
- Short uppercase labels: .625rem to .75rem with .075em to .105em tracking.
- Data uses tabular numerals.
- No monospace family. Alignment should come from grids and tabular figures.

## Spacing and geometry

- 4px base rhythm: 4, 8, 12, 16, 24, 28, 32, 48.
- Mobile side gutters: 16px; 12px at widths below 350px only where required.
- Touch targets: 44px minimum.
- Small radius: 6px. Medium radius: 10px. Full radius only for segmented controls.
- Avoid nested containers. Use rules, spacing, and alignment before surfaces.
- Safe areas are mandatory for top, bottom, and fixed actions.

## Mobile shell and navigation

- Product width: 320px to 430px.
- Bottom navigation contains Home, Progress, and Settings in production.
- Logging remains a focused drill-in flow rather than becoming a fourth persistent tab.
- Current navigation uses accent icon and label plus text/shape state, not colour alone.
- A fixed primary action may sit above navigation where repeated access justifies it.
- Fixed areas must have matching content padding so no content becomes unreachable.

## Home

Priority order:

1. Liftd and current date.
2. Primary Log exercise action.
3. Favourites / examples for quick entry.
4. Today's recorded exercises.
5. Latest previous session.

Use one visually dominant last/today section and compact quick-log rows. Do not wrap every log entry or summary in a standalone card. Demo content must be clearly labelled but visually subordinate.

## Exercise selection

- Search is a full-width labelled control with a 48px minimum height.
- Favourites and categories use section rules and dense 48px rows.
- Exercise identity is text-led: the full exercise name comes first, with equipment and category as quiet metadata where space allows. Do not use initials, abbreviations, or decorative tiles as the primary identifier. Stored emoji remains in data for compatibility only.
- Favourite toggles retain visible star semantics, pressed state, and accessible labels.

## Logging

The table structure is the centre of the system:

```text
SET | PREVIOUS | KG | REPS | DONE
```

- Previous performance stays adjacent to the current inputs.
- Only the current row is elevated and accented.
- Completed rows mute their values and show a checked box plus accessible completed label.
- Upcoming rows stay borderless with quiet separators.
- Weight and reps use numeric keyboards, visible labels, tabular figures, and persistent values.
- Set completion must update workout totals and move active emphasis forward.
- Add set is secondary. Save workout is persistent above the safe area.
- Edit mode uses a text badge and warning tone; deletion stays visibly separate and requires confirmation.

## Progress

- Exercise selector first, then one dominant estimated 1RM story.
- Chart sits directly on the base surface, not inside another card.
- Range selection uses a compact segmented control.
- Explanation follows the chart and names progressing, declining, plateau, or first-session state in text.
- Supporting records/history use aligned rows; avoid equally weighted metric-card grids.
- Positive and negative change include signs, words, or arrows so colour is never the only signal.

## Settings and data safety

- Settings are grouped by rules and spacing instead of repeated cards.
- Selected units and increments use filled segmented controls.
- Export/import remain ordinary rows with visible action icons.
- Device-only storage warning remains prominent but compact.
- Destructive actions use the danger colour and existing confirmation modal.

## Modals and overlays

- Bottom sheet on mobile, constrained to the mobile shell.
- Strong backdrop, solid raised surface, no decorative blur.
- Native focus management and Escape support where practical.
- Primary and destructive actions use the same height and geometry as the main app.

## Interaction states

Every control supports default, focus-visible, active, disabled, error, and success where relevant.

- Focus ring: 3px `--color-focus`, 2px offset.
- Press feedback: surface lightness change plus maximum scale of .985.
- Disabled: reduced contrast but still legible; cursor and semantics disabled.
- Error: danger outline plus nearby text, never border colour alone.
- Success: check/text plus success or accent colour.

## Motion

- 160ms to 220ms, ease-out-quart or ease-out-quint.
- Motion communicates navigation, completion, or modal state only.
- No page-load choreography, bounce, or elastic motion.
- `prefers-reduced-motion: reduce` collapses all non-essential durations.

## Accessibility

- WCAG AA contrast for text and controls.
- Browser zoom remains enabled; never use `user-scalable=no`.
- Visible labels for inputs; placeholders are supplementary.
- 44px minimum touch targets.
- Semantic headings, buttons, status text, and `aria-pressed` / `aria-current` where appropriate.
- State is never communicated by colour alone.

## Anti-patterns

- Pure black or white.
- Emoji-led exercise identity.
- Repeated identical cards.
- Glassmorphism, decorative gradients, neon glows, and oversized radii.
- A dashboard of equally weighted metrics.
- Hidden gesture-only actions.
- Accent colour on inactive content.

## Validation status

The prototype was checked at 320 × 700 and 390 × 844. Production implementation must repeat those checks with empty, demo, populated, editing, modal, and progress states. Accent intensity and home density may be tuned during implementation QA without changing this direction.
