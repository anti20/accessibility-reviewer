# Example Prompts

## General Component Review

```text
Use $accessibility-reviewer to review this component for accessibility. Prioritize issues that affect screen reader users, keyboard users, text scaling, touch targets, and WCAG-related risks.
```

## Specific File

```text
Use $accessibility-reviewer to review src/components/LoginForm.tsx. Focus on screen reader behavior, keyboard navigation, error messages, labels, and color contrast risks.
```

## Changed Files Before a PR

```text
Use $accessibility-reviewer to review the changed UI files in this project. Prioritize blockers for blind users, keyboard/focus issues, missing labels, text scaling problems, and provide fixed code where possible.
```

## React

```text
$accessibility-reviewer review src/components/SearchToolbar.tsx. Pay special attention to icon-only buttons, aria-expanded, focus order, and visible focus states.
```

## React Native

```text
$accessibility-reviewer review app/screens/ProfileScreen.tsx. Check TalkBack/VoiceOver labels, accessibilityRole, accessibilityState, hitSlop, target size, and allowFontScaling usage.
```

## SwiftUI

```text
Use $accessibility-reviewer to review MyScreen.swift. Pay special attention to VoiceOver, Dynamic Type, tap targets, custom gestures, and whether native Button/Toggle controls should be used.
```

## UIKit Swift

```text
Use $accessibility-reviewer to review SettingsViewController.swift. Focus on accessibilityLabel, accessibilityTraits, Dynamic Type, VoiceOver element order, and custom image/button controls.
```

## Flutter

```text
Use $accessibility-reviewer to review lib/screens/settings_screen.dart and provide fixed code only where it improves accessibility. Check TalkBack, Semantics, Tooltip, text scaling, and tap target size.
```

## Claude/Cursor-Style Agents

```text
/accessibility-reviewer review the currently open file. Return severity-ranked accessibility issues and a refactored version only if useful.
```

```text
Use the Accessibility Reviewer skill to audit the components in ./src/components. Start with the highest-risk interactive components and avoid generic findings.
```
