# Sample Output

This sample is a quick self-test using a SwiftUI component with common accessibility issues.

Self-test input:

```swift
struct MessageRow: View {
    let title: String
    let subtitle: String
    let unread: Bool
    let delete: () -> Void

    var body: some View {
        HStack {
            Circle()
                .fill(unread ? Color.blue : Color.gray)
                .frame(width: 10, height: 10)

            VStack(alignment: .leading) {
                Text(title)
                    .font(.system(size: 16))
                    .lineLimit(1)
                    .frame(height: 20)

                Text(subtitle)
                    .font(.system(size: 13))
                    .foregroundColor(.gray)
            }

            Spacer()

            Image(systemName: "trash")
                .foregroundColor(.red)
                .onTapGesture {
                    delete()
                }
        }
        .padding(8)
    }
}
```

# Accessibility Review

## Verdict

Review score: 58/100
Overall risk: High
Main problem: The destructive action is gesture-only and key text can fail Dynamic Type.
Recommended action: Replace the tappable image with a native Button and make text/layout scale.

## Issues to fix

### 1. [High] Delete action is a gesture-only image

File: MessageRow.swift
Line: 26
Problem: `Image(systemName: "trash")` uses `onTapGesture` instead of a native button.
Why it matters: VoiceOver may not expose it as a button, and users may not understand or reliably activate the destructive action.
Recommended fix: Replace the image gesture with a `Button` using a destructive role and an accessible label.
WCAG/platform notes: Robust / 4.1.2 Name, Role, Value; iOS VoiceOver button semantics and traits.

Original code:

```swift
Image(systemName: "trash")
    .foregroundColor(.red)
    .onTapGesture {
        delete()
    }
```

Fixed code:

```swift
Button(role: .destructive, action: delete) {
    Image(systemName: "trash")
}
.buttonStyle(.borderless)
.accessibilityLabel("Delete message")
```

### 2. [High] Title text can clip at larger Dynamic Type sizes

File: MessageRow.swift
Line: 14
Problem: The title uses a fixed font size, one-line truncation, and a fixed height.
Why it matters: Users who rely on larger text may lose the message title or see clipped content.
Recommended fix: Use a semantic text style and allow the title to wrap vertically.
WCAG/platform notes: Perceivable / 1.4.10 Reflow; iOS Dynamic Type.

Original code:

```swift
Text(title)
    .font(.system(size: 16))
    .lineLimit(1)
    .frame(height: 20)
```

Fixed code:

```swift
Text(title)
    .font(.body)
    .lineLimit(nil)
    .fixedSize(horizontal: false, vertical: true)
```

### 3. [Medium] Unread state relies on color alone

File: MessageRow.swift
Line: 9
Problem: The unread state is communicated only by a blue or gray circle.
Why it matters: Screen reader users and some low-vision users may miss whether the message is unread.
Recommended fix: Hide the decorative circle and expose unread state in the row's accessibility label or value.
WCAG/platform notes: Perceivable / 1.4.1 Use of Color; iOS VoiceOver state communication.

Original code:

```swift
Circle()
    .fill(unread ? Color.blue : Color.gray)
    .frame(width: 10, height: 10)
```

Fixed code:

```swift
Circle()
    .fill(unread ? Color.blue : Color.gray)
    .frame(width: 10, height: 10)
    .accessibilityHidden(true)
```

Add the state to the row:

```swift
HStack {
    // row content
}
.accessibilityElement(children: .combine)
.accessibilityLabel("\(title), \(subtitle)")
.accessibilityValue(unread ? "Unread" : "Read")
```

Would you like me to apply the fixes now?

Options:
- Apply all recommended fixes
- Apply only selected issue numbers
- Do not apply fixes; leave this as a manual review
