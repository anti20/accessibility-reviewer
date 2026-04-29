# Accessibility Reviewer Agent Skill

Accessibility Reviewer is an agent skill that helps AI coding assistants review frontend and mobile UI components for accessibility risks and practical refactoring opportunities.

It focuses on the mistakes that matter most for blind and visually impaired users: missing names and roles, broken keyboard access, unclear screen reader output, weak Dynamic Type and text scaling support, touch target problems, color-only cues, status messages, modals, and custom controls that should probably be native controls.

The skill supports React, React Native, SwiftUI, UIKit Swift, plain Swift UI-related code, and Flutter. It uses the Agent Skills format, so it is designed to work with Codex and other skill-aware coding agents. The repository also includes a Claude Code plugin manifest for compatible Claude/Cursor-style plugin workflows.

## Installing Accessibility Reviewer

You can install this skill into Codex, Claude Code, Cursor-style agents, and other compatible tools using an Agent Skills installer:

```bash
npx skills add https://github.com/anti20/accessibility-reviewer --skill accessibility-reviewer
```

Some installers use `add-skill` instead:

```bash
npx add-skill https://github.com/anti20/accessibility-reviewer --skill accessibility-reviewer
```

If `npx` is not available, install Node.js first. On macOS with Homebrew:

```bash
brew install node
```

You can also clone this repository and copy the skill folder manually:

```bash
git clone https://github.com/anti20/accessibility-reviewer.git
mkdir -p ~/.codex/skills
cp -R accessibility-reviewer/accessibility-reviewer ~/.codex/skills/accessibility-reviewer
```

## Using Accessibility Reviewer in Codex

In Codex, trigger the skill directly:

> $accessibility-reviewer review src/components/LoginForm.tsx

You can also use natural language:

> Use the accessibility-reviewer skill to review the changed UI files in this project. Prioritize blockers for blind users, keyboard/focus issues, missing labels, text scaling problems, and provide fixed code where possible.

For focused reviews:

> $accessibility-reviewer review lib/screens/settings_screen.dart. Focus on TalkBack, target size, and text scaling.

## Using Accessibility Reviewer in Claude, Cursor, and Similar Agents

If your agent supports Agent Skills, install the skill folder and invoke it using that tool's skill syntax. Common patterns include:

> /accessibility-reviewer review this component for VoiceOver and Dynamic Type issues.

> Use the Accessibility Reviewer skill on the currently open file. Return issues and fixed code only where it improves accessibility.

Claude Code plugin-compatible tools can read `.claude-plugin/plugin.json`, which points to the `accessibility-reviewer` skill folder in this repository. If your tool expects skills under a project or user-level skills directory, copy the `accessibility-reviewer/` folder there.

## Example Prompts

See [examples/prompts.md](examples/prompts.md) for ready-to-use prompts for React, React Native, SwiftUI, UIKit, Flutter, changed-file review, and pre-PR checks.

## Sample Output

See [examples/sample-output.md](examples/sample-output.md) for a representative review format with a review score, overall risk, prioritized numbered issues, concise fixes, WCAG/platform notes, and original/fixed code suggestions where possible.

## Report Format

Final reports start with `# Accessibility Review`, followed by a short verdict block:

- `Review score`
- `Overall risk`
- `Main problem`
- `Recommended action`

Issues appear under `## Issues to fix` in numbered priority order. Each issue uses a short file, line, problem, why it matters, and recommended fix format. When a concrete fix is possible, the report includes `Original code:` and `Fixed code:` snippets.

Every completed review ends by asking whether to apply all fixes, apply only selected issue numbers, or leave the review as manual guidance. Progress updates are only for longer reviews while work is underway; they should not appear in the final report.

## What the Skill Checks

- WCAG-related risks: text alternatives, semantic structure, contrast, reflow, keyboard access, focus order, focus visibility, target size, labels, error identification, status messages, and names/roles/values.
- Screen reader behavior: accessible names, roles, traits, states, grouping, headings, live regions, modal behavior, decorative content, and duplicate announcements.
- Framework-specific pitfalls: custom clickable views, icon-only buttons, gesture-only actions, hidden focusable content, suppressed font scaling, clipped text, and fixed-size layouts.
- UX clarity: ambiguous copy, visual-only state, inaccessible empty/error/loading states, and unclear destructive actions.
- Refactoring opportunities: native semantic controls, stable labels, state-specific accessibility props, and small targeted changes instead of broad rewrites.
- Long-review progress updates: short status phrases such as "Reading project files", "Checking accessibility", and "Writing report" before the final report.

## Repository Layout

```text
accessibility-reviewer/
├── .claude-plugin/
│   └── plugin.json
├── accessibility-reviewer/
│   ├── SKILL.md
│   ├── agents/
│   │   └── openai.yaml
│   └── references/
│       └── framework-checklists.md
├── examples/
│   ├── prompts.md
│   └── sample-output.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Contributing

Contributions are welcome. Useful changes include sharper framework checks, better sample prompts, corrected WCAG mappings, and examples from real UI accessibility failures.

Please keep the skill concise. Skills spend context every time they are loaded, so prefer high-signal guidance, small examples, and framework-specific surprises over generic accessibility advice.

See [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

Accessibility Reviewer is available under the [MIT License](LICENSE).
