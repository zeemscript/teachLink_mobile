# Pull Request: [Short, descriptive title]

## Summary

[Describe the changes in detail. What is being changed and why? Include enough context for reviewers to understand the impact without reading the diff.]

## Motivation

[Explain the problem this PR solves or the feature it adds. Reference any related issues, user feedback, or business requirements. If this is a bug fix, describe the symptoms and impact.]

## Type of Change

Select all that apply:

- [ ] **Bug fix** — Non-breaking change which fixes an issue
- [ ] **New feature** — Non-breaking change which adds functionality
- [ ] **Breaking change** — Fix or feature that would cause existing functionality to not work as expected
- [ ] **Chore / Refactor** — No functional changes
- [ ] **Documentation** — Documentation updates only
- [ ] **Governance** — Changes to governance documents only

## Testing Done

Describe how you verified this change works correctly:

- [ ] **Unit tests** — List the test files or describe test coverage added
- [ ] **Integration tests** — Describe integration testing performed
- [ ] **Manual verification** — Describe manual testing steps (e.g., iOS/Android UI checks, specific user flows)
- [ ] **Regression tests** — Describe testing for related areas to ensure no unintended side effects

## Security Considerations

Address the following if applicable to this change:

- [ ] **Data storage** — Does this store user data securely (e.g., avoiding plain AsyncStorage for sensitive data)?
- [ ] **Token handling** — Is token handling secure (no token exposure in logs or UI)?
- [ ] **Input validation** — Are all user inputs validated and sanitized?
- [ ] **Deep links** — Is deep link handling safe from malicious payloads?
- [ ] **Dependencies** — Are new dependencies vetted for security vulnerabilities?
- [ ] **N/A** — No security implications

## Performance Considerations

Address the following if applicable to this change:

- [ ] **React hooks** — Are React hooks (`useCallback`, `useMemo`) used appropriately to prevent unnecessary renders?
- [ ] **List optimization** — Is `FlatList` optimized (e.g., using `getItemLayout`, `keyExtractor`)?
- [ ] **Async patterns** — Are asynchronous patterns handled correctly (e.g., `useEffect` cleanup to avoid memory leaks)?
- [ ] **Bundle size** — Have bundle size impacts been considered?
- [ ] **Network calls** — Are network calls optimized (e.g., caching, debouncing)?
- [ ] **N/A** — No performance implications

## Linked Issues

List related issues this PR addresses or closes:

- Closes #[issue-number]
- Related to #[issue-number]
- Part of #[issue-number]

## Checklist

- [ ] I have read the [CONTRIBUTING](../../CONTRIBUTING.md) guide
- [ ] My code follows the project's style guidelines
- [ ] I have run the local quality checks (`npm run lint`, `npm run typecheck`)
- [ ] I have updated the documentation accordingly
- [ ] Are there architectural changes? If so, is there an Architectural Decision Record (ADR)?
- [ ] Does this change require governance review or policy updates?
- [ ] I have reviewed the [Security Policy](../SECURITY_POLICY.md) if this change has security implications
