---
name: accessibility-reviewer
description: Review frontend and mobile UI components for accessibility risks and refactoring opportunities. Use when Codex is asked to audit, review, fix, or improve React, React Native, SwiftUI, UIKit Swift, plain Swift UI-related code, or Flutter components for WCAG-related risks, blind and visually impaired users, screen reader support, semantic roles, labels, hints, color contrast, touch targets, keyboard/focus navigation, Dynamic Type/text scaling, UX clarity, performance, or code quality.
---

# Accessibility Reviewer

## Overview

Use this skill to review UI component code with an accessibility-first lens, especially for blind and visually impaired users. Prioritize actionable, evidence-based findings; map each meaningful risk to a WCAG category or platform rule; include severity; and provide fixed-code suggestions whenever a concrete fix is possible.

## Workflow

1. Identify the framework and platform from file names, imports, syntax, and user context.
2. Inspect the component's rendered behavior, not only the code shape: interactive elements, state changes, text alternatives, focus order, gestures, visual-only cues, error states, loading states, disabled states, and navigation.
3. Load [references/framework-checklists.md](references/framework-checklists.md) for framework-specific API names and pitfalls when reviewing React, React Native, SwiftUI, UIKit Swift/plain Swift UI code, or Flutter.
4. Prefer severity-ranked, code-grounded issues over broad advice. Distinguish confirmed bugs from plausible risks that require runtime visual verification.
5. Apply the "do not over-report" rule: do not list speculative, generic, or duplicate findings. If the code already uses the platform's native accessible pattern correctly, say so briefly or omit it.
6. Apply the "refactor only when useful" rule: include refactored code only when there is a concrete fix, a risky pattern should be replaced, or code will clarify the recommendation. If no useful code change is needed, write "No refactored code needed" in that section.
7. When refactoring, keep the original architecture and style unless accessibility requires a structural change.
8. If runtime validation is possible and appropriate, suggest or run relevant checks: browser accessibility tree inspection, screen reader smoke test, keyboard-only traversal, text scaling, color contrast calculation, snapshot at large font sizes, or native accessibility inspector.

## Progress Updates

During longer project or folder reviews, send short progress updates before the final report. Use only one of these 1-4 word updates:

- Reading project files
- Finding UI components
- Checking accessibility
- Reviewing screen readers
- Checking visual risks
- Preparing fixes
- Writing report

When the final report is ready, stop sending progress updates. The final answer should contain only the completed report.

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

Start with this short verdict block:

```text
# Accessibility Review

## Verdict

Review score: X/100
Overall risk: Low / Medium / High / Critical
Main problem: short one-line summary
Recommended action: short one-line summary
```

Then list issues in priority order under `## Issues to fix`. Each issue must be numbered and short. Prefer concrete file and line references. Group duplicate issues when they have the same fix. Do not over-report minor issues.

Use this issue format:

```text
### 1. [Severity] Issue title

File: path/to/file.swift
Line: 42
Problem: short explanation
Why it matters: short explanation
Recommended fix: short instruction
WCAG/platform notes: brief category, success criterion, or platform rule when useful
```

Include `Original code` and `Fixed code` blocks whenever a concrete code fix is possible:

````text
Original code:

```swift
// original code
```

Fixed code:

```swift
// improved code
```
````

When no useful code change is warranted or the fix depends on unavailable surrounding code, briefly say that fixed code is not provided and explain why.

At the end of every completed review, ask exactly:

```text
Would you like me to apply the fixes now?

Options:
- Apply all recommended fixes
- Apply only selected issue numbers
- Do not apply fixes; leave this as a manual review
```

If the user chooses selected issue numbers, ask which issue numbers to apply.

## Refactoring Rules

- Preserve visible behavior unless the visible behavior is the accessibility problem.
- Always provide a fixed-code suggestion when the issue has a concrete local code fix.
- Keep fixed-code snippets focused on the smallest useful replacement, not a full-file rewrite unless the surrounding structure is necessary.
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

## Uncertainty Guide

- Direct evidence: The issue is visible in code or follows from a deterministic platform rule. Report it normally.
- Runtime-dependent evidence: The issue depends on styles, runtime state, theme tokens, localization, or composition. Label it as needing runtime verification.
- Plausible risk only: Keep it brief, avoid letting it dominate the review, and do not count it as a major score penalty unless the user asked for risk discovery.

## WCAG Category Mapping

WCAG started as web guidance, but many principles also apply to mobile and native apps. Use WCAG as a shared accessibility baseline, not as the only source of truth for SwiftUI, UIKit, React Native, or Flutter.

Use these WCAG categories to keep findings grounded:

- **Perceivable**: text alternatives, captions, semantic structure, color contrast, color-only indicators, text resize/reflow.
- **Operable**: keyboard access, focus order, focus visibility, target size, pointer gestures, motion/animation controls, bypass blocks.
- **Understandable**: clear labels, predictable interactions, instructions, validation, error identification, consistent navigation.
- **Robust**: name/role/value, valid semantics, status messages, compatibility with assistive technologies.

When useful, include success criteria numbers such as 1.1.1, 1.3.1, 1.4.3, 1.4.10, 1.4.11, 2.1.1, 2.4.3, 2.4.7, 2.5.5, 3.3.1, 3.3.2, 4.1.2, and 4.1.3. Do not force a criterion number if the mapping is uncertain; use the category instead.

For native mobile, also consider platform-specific rules and APIs:

- iOS VoiceOver, Dynamic Type, `accessibilityLabel`, `accessibilityHint`, and `accessibilityTraits`.
- Android TalkBack, target sizing, focus behavior, and platform semantic roles.
- Flutter `Semantics`, `Tooltip`, `ExcludeSemantics`, `MergeSemantics`, and focus traversal.
- React Native `accessibilityRole`, `accessibilityLabel`, `accessibilityHint`, `accessibilityState`, hit slop, and font scaling.

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
- The review score, overall risk, main problem, and recommended action are consistent with the findings.
- Every issue is numbered, concise, and ordered by priority.
- Every meaningful issue maps to a WCAG category or likely success criterion.
- Native mobile issues include platform-specific notes when WCAG alone is incomplete.
- Screen reader impact is explained from the user's perspective.
- Keyboard/focus, touch target, text scaling, and contrast risks were considered.
- Fixed code is provided whenever a concrete code fix is possible.
- The response avoids generic advice and duplicate findings.
- Any uncertainty is labeled as a runtime verification need.
- The final review ends with the required "Would you like me to apply the fixes now?" options.
