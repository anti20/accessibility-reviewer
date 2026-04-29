# Contributing

Thank you for helping improve Accessibility Reviewer.

Good contributions usually do one of these things:

- Add a framework-specific check that catches a real accessibility failure.
- Improve an example so agents produce more actionable findings.
- Tighten WCAG mappings without overstating certainty.
- Remove generic guidance that spends tokens without improving review quality.
- Add sample prompts or sample output that reflects real frontend or mobile work.

## Guidelines

- Keep `accessibility-reviewer/SKILL.md` concise and procedural.
- Put detailed framework examples in `accessibility-reviewer/references/`.
- Prefer native controls before ARIA or platform accessibility overrides.
- Include severity and confidence guidance when adding new review behavior.
- Avoid duplicate findings and broad accessibility advice that could apply to any component.
- Keep examples small enough for an agent to scan quickly.

## Testing Changes

Before opening a pull request, read the changed skill files as if you were an agent loading them for the first time:

1. Confirm the trigger description still names the supported frameworks and review areas.
2. Confirm new guidance is actionable from code evidence.
3. Confirm examples do not encourage misleading labels, noisy announcements, or unnecessary custom accessibility code.
4. Try at least one prompt from `examples/prompts.md` against a real component.

## License

By contributing, you agree that your contribution will be licensed under the MIT License.
