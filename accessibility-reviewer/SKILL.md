---
name: accessibility-reviewer
description: Review frontend and mobile UI components for accessibility risks and refactoring opportunities. Use when Codex is asked to audit, review, fix, or improve React, React Native, SwiftUI, UIKit Swift, plain Swift UI-related code, or Flutter components for WCAG-related risks, blind and visually impaired users, screen reader support, semantic roles, labels, hints, color contrast, touch targets, keyboard/focus navigation, Dynamic Type/text scaling, UX clarity, performance, or code quality.
---

# Accessibility Reviewer

## Overview

Use this skill to review UI component code with an accessibility-first lens, especially for blind and visually impaired users. Prioritize actionable, evidence-based findings; map each meaningful risk to a WCAG category; include severity and confidence; and only provide refactored code when it materially helps the user fix or understand the issue.

## Workflow

1. Identify the framework and platform from file names, imports, syntax, and user context.
2. Inspect the component's rendered behavior, not only the code shape: interactive elements, state changes, text alternatives, focus order, gestures, visual-only cues, error states, loading states, disabled states, and navigation.
3. Load [references/framework-checklists.md](references/framework-checklists.md) for framework-specific API names and pitfalls when reviewing React, React Native, SwiftUI, UIKit Swift/plain Swift UI code, or Flutter.
4. Prefer severity-ranked, code-grounded issues over broad advice. Distinguish confirmed bugs from plausible risks that require runtime visual verification.
5. Apply the "do not over-report" rule: do not list speculative, generic, or duplicate findings. If the code already uses the platform's native accessible pattern correctly, say so briefly or omit it.
6. Apply the "refactor only when useful" rule: include refactored code only when there is a concrete fix, a risky pattern should be replaced, or code will clarify the recommendation. If no useful code change is needed, write "No refactored code needed" in that section.
7. When refactoring, keep the original architecture and style unless accessibility requires a structural change.
8. If runtime validation is possible and appropriate, suggest or run relevant checks: browser accessibility tree inspection, screen reader smoke test, keyboard-only traversal, text scaling, color contrast calculation, snapshot at large font sizes, or native accessibility inspector.

## Review Priorities

Check these areas before commenting on lower-value style issues:

- WCAG-related risks: perceivable text alternatives, meaningful sequence, color use, contrast, resize/reflow, keyboard access, focus order, visible focus, target size, input labels, error identification, status messages, names/roles/values, and consistent navigation.
- Screen reader support: accessible names, roles, traits, state announcements, grouping, headings, live regions/status updates, modal focus containment, decorative content hiding, and avoiding duplicate or misleading announcements.
- Semantics: use native controls first; avoid clickable non-controls; expose button/link/toggle/image/header/list semantics matching the UI behavior.
- Labels and hints: make labels specific and stable; use hints only for extra behavior, not as a substitute for names; include selected/expanded/disabled/error/loading state when relevant.
- Color and visual cues: flag low contrast, color-only state, placeholder-only labels, icon-only actions without names, and custom focus indicators that may disappear.
- Touch targets: check at least 44 x 44 CSS px/pt/dp where platform conventions allow; flag tightly packed controls and gesture-only actions without alternatives.
- Keyboard and focus: verify tab order, focusability, Escape/back behavior, roving tabindex where appropriate, focus restoration, traps, skip affordances, and custom controls.
- Dynamic Type/text scaling: avoid fixed heights, clipped text, disabled font scaling, single-line truncation of essential labels, and layout that breaks at larger sizes.
- UX clarity: check ambiguous copy, unlabeled icons, hidden destructive actions, unclear validation, loading/progress silence, and inaccessible empty/error states.
- Performance and code quality: avoid excessive re-renders/state churn in accessibility props, unstable IDs, duplicated labels, expensive layout work, hidden-but-focusable elements, and custom controls where native controls would be simpler.

## Output Format

Always structure the answer with these sections, omitting only sections that are truly not applicable:

1. **Verdict**: State whether the component passes, needs changes, or has serious accessibility blockers. Include the top severity present and the main reason in one or two sentences.
2. **Accessibility issues**: List concrete issues, ordered by severity. For each issue include `Severity`, `Confidence`, `Evidence`, `Impact`, and `Fix`.
3. **WCAG-related risks**: Map issues to likely WCAG categories or success criteria. Say "likely" when exact conformance cannot be proven from static code.
4. **Screen reader issues**: Describe how VoiceOver, TalkBack, NVDA, JAWS, or browser screen readers may experience the UI.
5. **UX/UI improvements**: Recommend interaction, content, layout, and state improvements for clarity and inclusiveness.
6. **Performance/code issues**: Call out maintainability and runtime concerns that affect accessibility or UI quality.
7. **Refactored code**: Provide a focused replacement or patch-style snippet only when useful. Keep it concise and idiomatic for the detected framework; otherwise state that no refactored code is needed.
8. **Short explanation of changes**: Summarize what changed and why.

Use this compact issue format when possible:

```text
- [High | Confidence: High] Missing accessible name on icon-only button
  WCAG: Perceivable / 1.1.1 Non-text Content; Operable / 4.1.2 Name, Role, Value
  Evidence: <button><SearchIcon /></button>
  Impact: Screen reader users may hear only "button" with no purpose.
  Fix: Add a visible label or an accessible name; hide the decorative icon.
```

## Refactoring Rules

- Preserve visible behavior unless the visible behavior is the accessibility problem.
- Prefer native semantic elements/components before ARIA or platform accessibility overrides.
- Do not add ARIA, accessibility props, labels, or hints that disagree with visible behavior.
- Avoid making decorative icons/images accessible; hide them from assistive technologies.
- Keep accessible names stable across state changes unless the control's purpose changes.
- Expose state separately from name when the framework supports it.
- Ensure disabled, loading, selected, expanded, checked, invalid, and required states are communicated.
- Keep keyboard and screen reader order aligned with visual and logical order.
- Avoid "click here", vague labels, and labels that only make sense visually.
- For contrast, provide a risk callout if exact colors/theme tokens are unavailable; calculate ratios when exact foreground/background values are present.
- Do not convert every component into a custom accessibility implementation. Native controls with correct visible labels often need fewer props, not more.
- Do not use `aria-label`, `accessibilityLabel`, or `Semantics(label:)` to hide unclear visible UX. Prefer improving visible text when possible.

## Severity Guide

- **Critical**: Prevents core task completion for keyboard or screen reader users, traps focus, hides essential information, or makes primary controls unnamed/unusable.
- **High**: Substantially impairs comprehension or operation, such as missing input labels, incorrect interactive roles, inaccessible modals, critical low contrast, or broken Dynamic Type on essential flows.
- **Medium**: Creates meaningful friction or ambiguity, such as weak hints, inconsistent headings, small targets, missing status announcements, or unclear validation.
- **Low**: Improves polish, resilience, or maintainability without blocking access, such as clearer labels, cleaner grouping, or reducing redundant announcements.

## Confidence Guide

- **High**: The issue is directly visible in the code or follows from a deterministic platform rule.
- **Medium**: The issue is likely from the code but depends on surrounding styles, runtime state, theme tokens, localization, or component composition.
- **Low**: The issue is a plausible risk that needs visual, device, assistive technology, or runtime verification. Keep low-confidence findings brief and do not let them dominate the review.

## WCAG Category Mapping

Use WCAG categories to keep findings grounded:

- **Perceivable**: text alternatives, captions, semantic structure, color contrast, color-only indicators, text resize/reflow.
- **Operable**: keyboard access, focus order, focus visibility, target size, pointer gestures, motion/animation controls, bypass blocks.
- **Understandable**: clear labels, predictable interactions, instructions, validation, error identification, consistent navigation.
- **Robust**: name/role/value, valid semantics, status messages, compatibility with assistive technologies.

When useful, include success criteria numbers such as 1.1.1, 1.3.1, 1.4.3, 1.4.10, 1.4.11, 2.1.1, 2.4.3, 2.4.7, 2.5.5, 3.3.1, 3.3.2, 4.1.2, and 4.1.3. Do not force a criterion number if the mapping is uncertain; use the category instead.

## Do Not Over-Report

- Do not report a missing label when visible text already gives the native control a correct accessible name.
- Do not report every decorative icon; report decorative icons only when exposed incorrectly or causing duplicate/noisy announcements.
- Do not report contrast without enough color information unless it is a clear risk from explicit colors or design tokens.
- Do not treat absence of ARIA as a problem when native semantics are sufficient.
- Do not list the same root cause in multiple sections as separate issues; cross-reference it instead.
- Prefer three high-signal findings over ten generic observations.

## Final Checklist

Before finalizing a review, verify:

- The verdict reflects the highest true severity.
- Every issue has severity and confidence.
- Every meaningful issue maps to a WCAG category or likely success criterion.
- Screen reader impact is explained from the user's perspective.
- Keyboard/focus, touch target, text scaling, and contrast risks were considered.
- Refactored code is included only when it helps.
- The response avoids generic advice and duplicate findings.
- Any uncertainty is labeled as a runtime verification need.
