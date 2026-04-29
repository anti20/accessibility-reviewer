# Sample Output

# Accessibility Review

## Verdict

Accessibility score: 68/100
Risk level: High
Main problem: Primary actions are not reliably exposed to screen reader and keyboard users.
Recommended action: Replace custom interactive markup with native controls and add missing accessible names.

## Issues to fix

### 1. [High] Icon-only close button has no accessible name

File: src/components/DialogActions.tsx
Line: 8
Problem: The close button contains only an icon, so it may be announced as an unnamed button.
Why it matters: Screen reader users may not know which control dismisses the dialog.
Recommended fix: Add a stable accessible name to the button and hide the decorative icon.
WCAG/platform notes: Perceivable / 1.1.1 Non-text Content; Robust / 4.1.2 Name, Role, Value.

Before:

```tsx
<button onClick={onClose}>
  <X />
</button>
```

After:

```tsx
<button type="button" aria-label="Close dialog" onClick={onClose}>
  <X aria-hidden="true" focusable="false" />
</button>
```

### 2. [High] Submit action uses a clickable non-control

File: src/components/DialogActions.tsx
Line: 12
Problem: A `div` is used as an interactive submit control.
Why it matters: Keyboard and screen reader users may not be able to focus or activate it consistently.
Recommended fix: Use a native submit button.
WCAG/platform notes: Operable / 2.1.1 Keyboard; Robust / 4.1.2 Name, Role, Value.

Before:

```tsx
<div className="submit" onClick={submit}>
  Submit
</div>
```

After:

```tsx
<button type="submit">
  Submit
</button>
```

### 3. [Medium] Saving state may not be announced

File: src/components/DialogActions.tsx
Line: 16
Problem: The visible text changes to "Saving..." but the status is not exposed as a live update.
Why it matters: Screen reader users may not know that submission started or completed.
Recommended fix: Add a polite status region for important async state changes.
WCAG/platform notes: Robust / 4.1.3 Status Messages.

Would you like me to apply the fixes now?

Options:
- Apply all recommended fixes
- Apply only selected issue numbers
- Do not apply fixes; leave this as a manual review
