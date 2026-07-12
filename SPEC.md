# Spec: Performance Ledger redesign

> Confirmed from the approved mobile prototype and Richard's instruction to uplift the production app.

**Status:** done
**Date:** 2026-07-12

## Goal

Move Liftd from a generic dark card interface to the approved Performance Ledger language while preserving its fast, offline-first progressive-overload workflow. The result should feel like a precise mobile training instrument and make previous performance, active inputs, and progress easier to scan between sets.

## Scope

- **In:** Project-level product and design documentation.
- **In:** New colour, typography, spacing, elevation, navigation, control, list, chart, modal, empty-state, and interaction language.
- **In:** Redesigned Home, exercise selection, logging/editing, Progress, Settings, and confirmation surfaces.
- **In:** Dense mobile set logging with previous performance adjacent to weight and repetitions.
- **In:** Existing localStorage data, custom exercises, favourites, edit/delete, progress explanations, import/export, settings, demo states, and PWA behaviour.
- **In:** Mobile QA at 320px and 390px widths plus a centred preview shell on larger screens.
- **Out:** React, npm, a build step, backend, accounts, cloud sync, social, recovery, coaching, workout planning, or new data models.
- **Out:** Commit, push, or deployment unless requested separately.

## Constraints

- Keep all application HTML, CSS, and JavaScript in `index.html`.
- Keep Chart.js as the only runtime dependency.
- Preserve the four existing localStorage keys and their schemas.
- Mobile-only product layout, 320px to 430px; larger viewports only centre the mobile shell.
- WCAG AA contrast, browser zoom enabled, visible focus states, reduced-motion support, and 44px minimum touch targets.
- The acid accent is functional, not decorative. It marks primary actions, selection, active input, completion, and personal records.
- Never rely on colour alone for state.
- Destructive actions retain confirmation.
- Bump the service-worker cache to `gym-tracker-2026-07-12`.

## Acceptance

- The production app visibly matches the approved Performance Ledger direction across every screen.
- The active set is the only elevated row; previous, weight, reps, and completion remain legible at 320px.
- Home prioritises last workout, quick access, today's entries, and weekly context without a repetitive card grid.
- Progress presents one dominant estimated 1RM story, an unboxed chart, supporting explanation, and record/history data with clear hierarchy.
- All current features continue to work with existing localStorage data.
- No horizontal scrolling, clipped controls, console errors, broken Chart.js rendering, or content hidden permanently behind fixed actions.
- Service-worker cache and cached font dependencies match the new implementation.

## Open questions

None blocking. Accent intensity and home density may be tuned during browser QA without changing the approved direction.
