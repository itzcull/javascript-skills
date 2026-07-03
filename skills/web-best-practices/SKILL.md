---
name: web-best-practices
description: Web interface quality guidelines for building, reviewing, and improving browser-based user interfaces. Use when working on frontend UX, accessibility, interactions, forms, animation, layout, content, performance, responsive behavior, or visual polish for web apps and sites.
license: MIT
metadata:
  author: itzcull
  version: "0.1.0"
  source: https://vercel.com/design/guidelines
---

# Web Best Practices

## Purpose

Use this skill to build and review high-quality web interfaces that are accessible, responsive, performant, resilient, and pleasant to use.

This skill is adapted from Vercel's Web Interface Guidelines. Treat the source URL as the authoritative reference when details need confirmation.

## When to Use

Use this skill when:

- Building or reviewing any browser-based UI.
- Implementing interactions, navigation, forms, dialogs, menus, tooltips, tables, lists, charts, or loading states.
- Improving accessibility, keyboard support, focus management, responsiveness, layout stability, or visual polish.
- Auditing generated UI code before shipping.
- Debugging UI quality problems such as layout shift, janky animation, bad mobile behavior, broken keyboard flows, or confusing form errors.

Do not use this as a substitute for project-specific design systems, product voice, accessibility requirements, or performance budgets. Apply local standards first when they are stricter or more specific.

## Operating Principles

- Prefer native browser semantics before custom behavior.
- Preserve standard browser affordances such as keyboard navigation, browser zoom, links, autofill, paste, Back/Forward, refresh, open-in-new-tab, and scroll restoration.
- Design every user state: empty, sparse, dense, loading, success, error, disabled, in-flight, offline, and permission-denied where relevant.
- Make interactions forgiving with generous targets, clear affordances, predictable focus, and recoverable destructive actions.
- Keep performance work user-centered: responsiveness, layout stability, fast feedback, and low input latency matter more than abstract micro-optimizations.
- Verify on real browser constraints, especially mobile Safari, desktop Safari, throttled CPU/network, zoom, and reduced motion.

## Interaction Checklist

- Ensure every flow works with keyboard only and follows WAI-ARIA Authoring Practices where applicable.
- Show a visible focus ring on every focusable element. Prefer `:focus-visible` and use `:focus-within` for grouped controls.
- Move, trap, and restore focus correctly in dialogs, menus, popovers, drawers, and multi-step flows.
- Match visual and hit targets. Use at least 24 px hit targets, and at least 44 px on mobile.
- Never disable browser zoom.
- Never block paste in `input` or `textarea`.
- Keep hydrated inputs from losing focus or value.
- Use `a` or framework link components for navigation. Do not replace navigational links with `button` or `div`.
- Persist shareable state in the URL when users expect refresh, sharing, Back/Forward, or deep links to work.
- Deep-link filters, tabs, pagination, expanded panels, and meaningful `useState` UI state.
- Restore prior scroll positions on Back/Forward navigation.
- Use optimistic updates when success is likely. On failure, show the error and roll back or offer Undo.
- Confirm destructive actions or provide Undo with a safe recovery window.
- Use `touch-action: manipulation` on controls when needed to avoid double-tap zoom issues.
- Avoid dead zones: if something looks interactive, the whole visible control should be interactive.
- Announce async updates such as toasts and inline validation with polite `aria-live` regions.
- Internationalize keyboard shortcuts for non-QWERTY layouts and show platform-specific symbols.

## Loading and Feedback

- For loading buttons, show progress while keeping the original label visible.
- Avoid spinner flicker by adding a short show delay around 150-300 ms and a minimum visible time around 300-500 ms.
- Use skeletons only when they mirror the final layout closely enough to avoid layout shift.
- Use ellipses for actions that require more input, such as `Rename…`, and for active progress states such as `Loading…`.
- Prefer inline help over tooltips. Use tooltips only when the information is supplementary and not required to complete the task.
- Design no dead ends. Every screen should offer a next step or recovery path.

## Animation Checklist

- Honor `prefers-reduced-motion` with a reduced-motion alternative.
- Prefer CSS animations, then the Web Animations API, then JavaScript animation libraries only when needed.
- Animate compositor-friendly properties such as `transform` and `opacity`.
- Avoid animating layout-affecting properties such as `width`, `height`, `top`, and `left` unless the tradeoff is deliberate.
- Never use `transition: all`; list the exact properties being animated.
- Animate only when it clarifies cause and effect or adds deliberate delight.
- Make animations interruptible by user input.
- Prefer input-driven animation over autoplay.
- Set transform origins to match where motion physically begins.
- For SVG transforms, animate `g` wrappers and set `transform-box: fill-box` and `transform-origin: center` for better browser consistency.

## Layout Checklist

- Align every element deliberately to a grid, baseline, edge, or optical center.
- Use optical alignment when perception beats geometry, including small 1 px adjustments.
- Balance text and icons in lockups by adjusting weight, size, spacing, or color.
- Verify layouts on mobile, laptop, and ultra-wide screens.
- Respect safe areas for notches and device insets with CSS `env()` variables.
- Fix unwanted overflow and excessive scrollbars. Test with always-visible scrollbars to catch Windows-style scrollbar issues.
- Prefer CSS flex, grid, and intrinsic layout over measuring layout in JavaScript.
- Let browser layout handle flow, wrapping, and alignment when possible.

## Content and Accessibility Checklist

- Keep page `<title>` accurate to the current context.
- Use semantic elements before ARIA: `button`, `a`, `label`, `table`, headings, and landmarks.
- Maintain a logical heading hierarchy and provide a skip-to-content link for substantial pages.
- Give every interactive control an accessible name.
- Provide descriptive `aria-label` values for icon-only buttons.
- Mark decorative icons with `aria-hidden`.
- Do not rely on color alone for status. Include text, icon shape, or another redundant cue.
- Ensure icon meaning is also available to non-sighted users.
- Keep accessible labels even when the visual design omits visible labels.
- Verify complex UI in the browser accessibility tree.
- Make user-generated content resilient: handle short, average, long, unbroken, and unexpected content.
- Format dates, times, numbers, delimiters, and currency for the user's locale.
- Prefer language settings such as `Accept-Language` and `navigator.languages` over IP or location for language selection.
- Mark brand names, product names, code tokens, and technical identifiers with `translate="no"` when browser translation would corrupt them.
- Use tabular numbers for numeric comparisons.
- Prefer typographic quotes and the single ellipsis character `…` when the product's content style allows it.
- Avoid widows, orphans, and awkward line breaks in prominent copy.
- Set `scroll-margin-top` on anchored headings so linked sections are not hidden under sticky UI.
- Use non-breaking spaces for glued terms such as values with units, keyboard shortcuts, and names.

## Form Checklist

- Every form control has a visible label or an assistive-tech label.
- Clicking a label focuses or toggles its associated control.
- Enter submits when there is a single text input or when focus is on the final logical control.
- In textareas, Enter inserts a newline and Cmd/Ctrl+Enter submits when shortcut submit is supported.
- Keep submit enabled until submission starts so users can trigger validation feedback.
- Disable submit during the in-flight request, show progress, and use an idempotency key when duplicate submissions would be harmful.
- Do not block typing. Allow input and show validation feedback instead of silently rejecting keystrokes.
- Do not pre-disable submit on incomplete forms unless there is a strong product reason.
- Show field errors next to their fields and focus the first error after failed submit.
- Use `autocomplete` and meaningful `name` attributes to support autofill.
- Use correct `type` and `inputmode` values for better keyboards and browser behavior.
- Set mobile input font size to at least 16 px to avoid iOS Safari auto-zoom on focus.
- Use placeholders only as examples or empty-state hints, not as labels.
- Warn before navigation when unsaved changes could be lost.
- Support password managers and 2FA flows. Allow pasting one-time codes.
- Avoid triggering password managers for non-auth fields with misleading names.
- Trim accidental trailing whitespace from input methods and text replacements before validation.
- Explicitly set `background-color` and `color` on native `select` controls to avoid Windows dark-mode contrast bugs.

## Performance Checklist

- Test in iOS Low Power Mode and macOS Safari for browser-specific constraints.
- Profile with browser extensions disabled when measuring performance.
- Use CPU and network throttling when profiling.
- Minimize re-renders and make unavoidable re-renders cheap.
- Keep keystroke paths cheap. Prefer uncontrolled inputs when controlled input loops become expensive.
- Batch DOM reads and writes to avoid unnecessary reflows and repaints.
- Move expensive long tasks off the main thread with Web Workers when interaction would otherwise block.
- Virtualize large lists or use `content-visibility: auto` where appropriate.
- Preload only above-the-fold images and lazy-load the rest.
- Set explicit image dimensions and reserve space to avoid image-caused CLS.
- Preconnect to important asset and CDN origins when the connection setup cost matters.
- Preload critical fonts and subset fonts to only the scripts, code points, and axes in use.
- Aim for mutation requests such as `POST`, `PATCH`, and `DELETE` to complete in under 500 ms when feasible.

## Visual Design Checklist

- Use layered shadows that model ambient and direct light rather than a single flat shadow.
- Combine borders and shadows for crisp edges when elevation or separation matters.
- Keep nested radii concentric: child radius should be less than or equal to parent radius.
- On non-neutral backgrounds, tint borders, shadows, and text toward the same hue.
- Use color-blind-friendly palettes for charts.
- Check contrast with perceptual tools such as APCA when possible.
- Increase contrast on interactive states such as hover, active, and focus.
- Match browser UI to page background with `theme-color` where appropriate.
- Set `color-scheme` on the root for dark themes so scrollbars and built-in controls have proper contrast.
- Avoid scaling text directly when animation causes anti-aliasing artifacts. Animate a wrapper instead.
- Watch for gradient banding, especially in dark fades and CSS masks.

## Copywriting Defaults

Use these defaults when the project has no stronger voice or content standard:

- Write in active voice.
- Be clear and concise.
- Use action-oriented labels.
- Keep nouns consistent and introduce as few terms as possible.
- Write to the user in second person.
- Use numerals for counts.
- Format currency consistently within a context.
- Separate numbers and units with a space, preferably a non-breaking space when they must stay together.
- Frame errors in a positive, recovery-oriented way.
- Error messages should explain what happened and how to fix it.
- Avoid vague button labels such as `Continue` when a specific action label such as `Save API Key` is available.

Vercel-specific preferences such as Title Case for product UI headings/buttons, sentence case on marketing pages, `&` instead of `and`, and placeholder formats should not override an existing product style guide.

## Review Workflow

1. Identify the user's goal, the interface surface, and the relevant states.
2. Check semantics, keyboard access, focus management, labels, and browser-native behavior first.
3. Check form behavior, validation, loading states, async feedback, and destructive action recovery.
4. Check responsive layout, safe areas, overflow, scroll behavior, and deep-link/share behavior.
5. Check animation purpose, reduced-motion behavior, and jank risk.
6. Check performance risks that affect interaction latency, layout stability, loading, or rendering.
7. Check visual polish, contrast, content clarity, and locale resilience.
8. Verify in browser when possible using keyboard navigation, mobile viewport, zoom, reduced motion, throttling, and accessibility tree inspection.

## Output

When applying this skill, produce the output that best fits the task:

- For implementation: code that preserves browser semantics, accessibility, responsive behavior, and performance constraints.
- For review: findings ordered by user impact, with concrete file or UI references and fixes.
- For design critique: prioritized issues grouped by interaction, accessibility, forms, layout, performance, and visual polish.
- For checklists: only include the sections relevant to the surface being built or reviewed.

## References

- Vercel Web Interface Guidelines: https://vercel.com/design/guidelines
- WAI-ARIA Authoring Practices: https://www.w3.org/WAI/ARIA/apg/patterns/
- MDN `inert`: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/inert
- MDN safe-area `env()`: https://developer.mozilla.org/en-US/docs/Web/CSS/env
- Chrome accessibility tree inspection: https://developer.chrome.com/blog/full-accessibility-tree
