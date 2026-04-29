# Framework Checklists

Use the relevant checklist for the detected framework. Treat these as prompts for review, not as exhaustive rules.

## React

- Prefer native `button`, `a`, `input`, `select`, `textarea`, `label`, `fieldset`, `legend`, `dialog`, `ul/ol/li`, `table`, and heading elements before ARIA.
- Flag clickable `div`/`span` elements unless they implement role, name, keyboard activation, focusability, disabled behavior, and visible focus.
- Check `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-expanded`, `aria-controls`, `aria-current`, `aria-selected`, `aria-pressed`, `aria-invalid`, `aria-required`, `aria-live`, and `role="status"` for correctness.
- Watch for `tabIndex` greater than 0, hidden focusable content, portals/modals without focus management, and custom menus/listboxes/tabs without expected keyboard behavior.
- Validate icon-only controls, SVG titles, decorative images with empty `alt`, informative images with useful `alt`, and loading/error announcements.
- Confirm CSS does not remove visible focus, rely only on color, clip text under zoom, or create low contrast.

Example:

```tsx
// Risk: unnamed icon-only button.
<button onClick={onClose}><X /></button>

// Better: native button with stable accessible name and decorative icon hidden.
<button type="button" aria-label="Close dialog" onClick={onClose}>
  <X aria-hidden="true" focusable="false" />
</button>
```

Example:

```tsx
// Risk: clickable non-control with incomplete keyboard behavior.
<div onClick={submit}>Submit</div>

// Better: native semantics and keyboard behavior.
<button type="submit">Submit</button>
```

## React Native

- Use `Pressable`, `Touchable*`, `Button`, `TextInput`, and native components with `accessibilityRole`, `accessibilityLabel`, `accessibilityHint`, `accessibilityState`, `accessibilityValue`, and `accessibilityLiveRegion` where appropriate.
- Prefer visible `Text` labels connected through clear grouping instead of relying on hints.
- Check `accessible`, `importantForAccessibility`, `accessibilityElementsHidden`, `accessibilityViewIsModal`, `onAccessibilityAction`, and custom action support.
- Flag interactive `View` components without role/name/action semantics or keyboard/TV accessibility where applicable.
- Check minimum target size around 44 x 44 dp/pt, adequate spacing, and alternatives for gesture-only interactions.
- Avoid `allowFontScaling={false}` on user-facing text unless there is a strong, documented exception. Check `maxFontSizeMultiplier` and layout resilience.

Example:

```tsx
// Risk: icon-only press target with no role/name/state.
<Pressable onPress={toggleFavorite}>
  <HeartIcon filled={favorite} />
</Pressable>

// Better: role, label, and state exposed.
<Pressable
  accessibilityRole="button"
  accessibilityLabel={favorite ? "Remove from favorites" : "Add to favorites"}
  accessibilityState={{ selected: favorite }}
  hitSlop={8}
  onPress={toggleFavorite}
>
  <HeartIcon filled={favorite} />
</Pressable>
```

## SwiftUI

- Prefer native `Button`, `Toggle`, `Link`, `TextField`, `Picker`, `Stepper`, `Slider`, `List`, `Section`, and `NavigationLink` before custom gesture-only views.
- Check `.accessibilityLabel`, `.accessibilityHint`, `.accessibilityValue`, `.accessibilityAddTraits`, `.accessibilityRemoveTraits`, `.accessibilityHidden`, `.accessibilityElement(children:)`, `.accessibilitySortPriority`, `.accessibilityFocused`, and `.accessibilityAction`.
- Use `.accessibilityElement(children: .combine)` for coherent grouped content and `.contain` when children should remain separately navigable.
- Flag `onTapGesture` used as a button without button semantics, tiny tap areas, decorative images not hidden, and text clipped by fixed frames.
- Respect Dynamic Type: avoid fixed heights for text containers, unnecessary `.lineLimit(1)`, hard-coded font sizes without scalable styles, and layouts that fail at accessibility text sizes.

Example:

```swift
// Risk: gesture-only custom button.
Image(systemName: "trash")
    .onTapGesture { deleteItem() }

// Better: native button semantics with a clear label.
Button(role: .destructive, action: deleteItem) {
    Image(systemName: "trash")
}
.accessibilityLabel("Delete item")
```

Example:

```swift
// Risk: fixed text container may clip at larger Dynamic Type sizes.
Text(title)
    .font(.system(size: 16))
    .frame(height: 20)

// Better: scalable text style and flexible layout.
Text(title)
    .font(.body)
    .fixedSize(horizontal: false, vertical: true)
```

## UIKit Swift and Plain Swift UI-Related Code

- Check `isAccessibilityElement`, `accessibilityLabel`, `accessibilityHint`, `accessibilityValue`, `accessibilityTraits`, `accessibilityIdentifier` misuse, `accessibilityElements`, `accessibilityFrame`, and `accessibilityViewIsModal`.
- Prefer `UIButton`, `UILabel`, `UITextField`, `UITextView`, `UISwitch`, `UISlider`, `UITableView`, `UICollectionView`, and `UIAccessibilityCustomAction` over custom-drawn controls.
- Verify `adjustsFontForContentSizeCategory`, `UIFontMetrics`, semantic text styles, `numberOfLines`, Auto Layout constraints, and content compression resistance for Dynamic Type.
- Check focus and announcements with `UIAccessibility.post(notification:argument:)` for screen changes, layout changes, and important status updates.
- Flag labels that are visually present but not programmatically associated with inputs, custom cells with poor element order, and controls smaller than 44 x 44 pt.

Example:

```swift
// Risk: custom image view used as a button without accessible behavior.
let closeIcon = UIImageView(image: UIImage(systemName: "xmark"))
closeIcon.isUserInteractionEnabled = true
closeIcon.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(close)))

// Better: native button with name and target size support.
let closeButton = UIButton(type: .system)
closeButton.setImage(UIImage(systemName: "xmark"), for: .normal)
closeButton.accessibilityLabel = "Close"
closeButton.addTarget(self, action: #selector(close), for: .touchUpInside)
```

Example:

```swift
// Risk: text will not scale with Dynamic Type.
label.font = UIFont.systemFont(ofSize: 14)

// Better: semantic text style with scaling enabled.
label.font = UIFont.preferredFont(forTextStyle: .body)
label.adjustsFontForContentSizeCategory = true
```

## Flutter

- Prefer built-in semantic widgets and controls: `ElevatedButton`, `TextButton`, `IconButton`, `Checkbox`, `Switch`, `Slider`, `TextField`, `DropdownButton`, `ListTile`, `Scaffold`, and `AppBar`.
- Use `Semantics`, `ExcludeSemantics`, `MergeSemantics`, `BlockSemantics`, `Focus`, `FocusTraversalGroup`, `Shortcuts`, `Actions`, and `Tooltip` where appropriate.
- Check `label`, `hint`, `button`, `link`, `header`, `selected`, `checked`, `enabled`, `expanded`, `liveRegion`, `textField`, `onTap`, and custom `SemanticsAction` usage.
- Flag `GestureDetector`/`InkWell` without semantic labels or keyboard/focus support, icon-only actions without `tooltip`, and decorative images exposed to assistive tech.
- Respect text scaling: do not suppress `MediaQuery.textScaler`/text scale, avoid fixed-height text containers, and test overflow at larger scale factors.
- Check target sizes against Material guidance, generally 48 x 48 logical pixels for tappable controls.

Example:

```dart
// Risk: icon-only action without a semantic label.
IconButton(
  icon: const Icon(Icons.close),
  onPressed: onClose,
)

// Better: tooltip supplies an accessible name for IconButton.
IconButton(
  tooltip: 'Close dialog',
  icon: const Icon(Icons.close),
  onPressed: onClose,
)
```

Example:

```dart
// Risk: custom tap target may lack semantics and keyboard behavior.
GestureDetector(
  onTap: submit,
  child: const Text('Submit'),
)

// Better: native button semantics, focus, and activation.
ElevatedButton(
  onPressed: submit,
  child: const Text('Submit'),
)
```
