# Spec: Rest timer (docked strip)

> From the 2026-08-06 feature brainstorm and the Mobbin-informed variant board in `rest-timer-variants.html`. Richard selected direction A, no notification, plus a settings toggle and a skip-driven off-ramp.

**Status:** built, verified at 320px and 390px, not yet deployed
**Date:** 2026-08-06

## Goal

Give the lifter a rest timer that starts itself when a set is completed and stays out of the way of the log. It should be readable at a glance between sets, dismissible in one thumb tap, and easy to turn off permanently for anyone who does not want it.

## Scope

- **In:** A docked rest strip on the log screen, pinned above `.log-save`, carrying a countdown, a drain bar, `+30s`, and `Skip`.
- **In:** Auto-start when a set is toggled to complete.
- **In:** Two new `gym_settings` fields: `restTimerEnabled` (bool, default `true`) and `restDuration` (seconds, default `90`).
- **In:** A Settings toggle row for the timer and a duration control (60 / 90 / 120 / 180 s) using the existing segmented-control pattern.
- **In:** An inline off-ramp after three consecutive skips in a session.
- **In:** Reduced-motion and screen-reader handling for a value that changes every second.
- **Out:** Sound, vibration, and Web Notifications. The timer is silent and visual only.
- **Out:** Per-exercise rest defaults. Global duration only in v1.
- **Out:** Persisting timer state across an app kill or hard reload.
- **Out:** Changes to the Progress tab, chart, navigation, or the set-completion motion already shipped.

## Behaviour

**Start.** Toggling a set to complete starts the timer at `restDuration`. Reopening a completed set does not start or reset it. Completing the final set still starts it, since another exercise usually follows.

**While running.** The strip shows `REST` and `m:ss`, with a hairline draining left to right. Restarting on a new completion resets to full duration rather than adding time. The set list stays fully interactive behind it.

**End.** At `0:00` the strip switches to a spent state (drain bar full, time reads `0:00` in muted treatment) and auto-hides after 5 seconds. No sound, no vibration, no notification. If the app was backgrounded, the countdown is recomputed from a stored `endsAt` timestamp on `visibilitychange`, so returning shows the true remaining time rather than a stalled one.

**Not persisted.** `endsAt` lives in `state`, not in `localStorage`. Killing the app ends the rest. This keeps the storage schema clean and matches the fact that a rest older than an app restart is not worth resuming.

**Skip off-ramp.** A per-session counter increments on each `Skip` and resets to zero whenever a rest is allowed to run to completion. On the third consecutive skip, the strip replaces its controls with a single inline offer: *"Skipping every rest? Turn the timer off."* with `Turn off` and `Keep it`. `Turn off` writes `restTimerEnabled: false` and hides the strip for good. `Keep it` dismisses. The offer appears at most once per session, and never again once dismissed in that session.

## Constraints

- All HTML, CSS, and JS stay in `index.html`.
- Reuse existing tokens. No new colours, radii, or easing values.
- The strip sits above `.log-save` (z-index 190) without covering it, and both must clear `env(safe-area-inset-bottom)`.
- `+30s`, `Skip`, and the off-ramp buttons are 44px minimum touch targets and must not sit close enough to `Save session` to be mis-hit.
- The ticking value carries `aria-live="off"`. A separate polite live region announces only "Rest started, 90 seconds" and "Rest complete", never the per-second tick.
- Under `prefers-reduced-motion`, the drain bar updates without a width transition.
- No `alert()`. The off-ramp is inline in the strip, not a modal.
- Turning the timer off in Settings removes the strip immediately, including mid-rest.

## Acceptance

- Completing a set starts the countdown; reopening one does not.
- `+30s` adds 30 seconds without restarting; `Skip` clears the strip immediately.
- The log stays scrollable and every set row stays editable while the timer runs.
- Backgrounding the PWA for 30 seconds and returning shows 30 fewer seconds, not a frozen clock.
- Reaching `0:00` produces no audio, no vibration, and no permission prompt.
- Three consecutive skips surface the off-ramp once; a completed rest in between resets the count.
- `Turn off` persists across a reload and the Settings toggle reflects it.
- With the timer disabled, completing a set behaves exactly as it does today.
- A screen reader announces start and completion only, not each second.
- No clipping, horizontal overflow, or console errors at 320px and 390px.

## Open questions

None blocking. Per-exercise rest defaults are the obvious v2 and are deliberately excluded here.

---

# Previous spec: Set-completion motion

> Confirmed from the 2026-07-17 motion-opportunity review and Richard's approval to implement its top recommendation.

**Status:** done
**Date:** 2026-07-17

## Goal

Make the core set-completion handoff legible without slowing workout logging: the completed check should register immediately and the next active row should arrive as the consequence of that action.

## Scope

- **In:** Completion feedback on the affected checkbox.
- **In:** A restrained entrance cue on only the newly active set row.
- **In:** Equivalent feedback when reopening a completed set.
- **In:** Rapid-toggle cleanup and a gentler reduced-motion variant.
- **In:** Mobile QA at 320px and 390px.
- **Out:** Progress dropdown motion, add/remove-set motion, chart animation, navigation changes, or new dependencies.

## Constraints

- Keep all application HTML, CSS, and JavaScript in `index.html`.
- Use only `transform` and `opacity`, with the existing 160ms `cubic-bezier(.22,1,.36,1)` motion vocabulary.
- Do not delay state updates, move focus, auto-scroll, animate unaffected rows, or persist transient motion state.
- Preserve the current localStorage schemas, accessibility state, and touch targets.
- Use a 120ms, 1px/0.94-scale reduced-motion variant rather than the standard cue.

## Acceptance

- Completing a set animates only its tick and the next active row.
- Reopening a set gives immediate feedback on that row.
- Completing the final set does not try to animate a nonexistent next row.
- Rapid repeated toggles do not queue motion or leave stale classes.
- Weight, rep, focus, add-set, and remove-set updates do not trigger this motion.
- The interaction has no clipping, horizontal overflow, console errors, or accessibility regressions at 320px and 390px.

## Open questions

None blocking.

---

# Previous spec: Performance Ledger redesign

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
