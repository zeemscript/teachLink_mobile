# Inclusive Language Guide

## Purpose

The words we use in code, documentation, issues, and conversation affect who feels
welcome in TeachLink Mobile. This guide says which terms the project prefers, which it
asks contributors to avoid, and how the guide is applied. It supports the expectations
in the [Code of Conduct enforcement policy](../COC_ENFORCEMENT.md) and the project's
[Values](../VALUES.md), and exists to make writing clearer as well as kinder: many of
the preferred terms are also more precise.

## Scope

This guide applies to everything contributors write in project spaces: source code
(identifiers, comments, strings, and test names), documentation, commit messages, pull
requests, issues, discussions, and release notes. It also applies to the app's own
user-facing text, which is read by teachers and children (see the
[Scope Statement](../SCOPE.md)).

It is a writing guide, not a list of banned words to be policed mechanically. Intent and
context matter, and the aim is to help people write well, not to catch them out.

## Principles

- **Be precise.** Prefer the term that says exactly what a thing is or does.
- **Describe the thing, not a person.** Avoid terms that carry a judgement about people
  or groups when a neutral technical term exists.
- **Plain language.** Avoid idioms and slang that do not translate across cultures and
  languages, which matters for a project with learners and contributors worldwide.
- **Assume good faith.** Correct gently, and expect to be corrected gently.

## Preferred Terms

| Instead of | Prefer | Why |
| --- | --- | --- |
| whitelist / blacklist | allowlist / denylist (or blocklist) | Names the function, not a colour |
| master / slave | primary / replica (or leader / follower, parent / child) | Removes the slavery metaphor and is more precise |
| master (branch) | `main` | The project's default branch is already `main` |
| sanity check | confidence check, quick check, validation | A "sanity" check can stigmatise mental health |
| dummy (value or data) | placeholder, sample, mock | Describes the purpose |
| grandfathered | legacy, exempt by prior approval | Avoids a term with a discriminatory origin |
| man-hours, manpower | person-hours, effort, staffing | Gender-neutral |
| guys (addressing a group) | everyone, folks, team, all | Includes everyone in the group |
| he / she (for an unknown person) | they, or rewrite in the plural | Gender-neutral |
| crazy, insane, lame, dumb (as intensifiers or insults) | surprising, unexpected, limited, unclear | Avoids using disability or mental health as an insult |
| blind spot, tone-deaf (as metaphors) | gap, oversight, unaware | Avoids metaphors built on disability |
| cripple (a feature or system) | disable, degrade, limit | Same |
| abort (for a process) | cancel, stop, exit | "Abort" is acceptable as a standard API name, see exceptions |
| hang, kill (a process) | stop, terminate, unresponsive | Acceptable as standard system terms, see exceptions |
| simple, easy, just, obviously (about a task) | omit, or describe the steps | These can make a reader feel inadequate when it is not simple for them |
| native speaker | fluent speaker | Avoids implying a hierarchy of speakers |

When writing about people, use the terms they use for themselves, and avoid referring to
a person's identity unless it is relevant.

## Terms to Avoid

Do not use, in any project space:

- slurs and demeaning terms about any group;
- terms that use a protected characteristic as an insult;
- gendered or ableist terms as jokes or intensifiers; and
- phrases that exclude or single out people, such as "you guys" to a mixed group, when a
  neutral phrase would serve.

Repeated or targeted use of such language is a Code of Conduct matter, handled under the
[enforcement policy](../COC_ENFORCEMENT.md) and reported through the
[reporting process](../COC_REPORTING.md).

## Exceptions

Some uses are not changed:

- **External names.** A third-party API, protocol, file format, command, or error message
  that uses a term we would avoid is quoted or called as-is. The code that wraps it
  should use the preferred term for its own names where practical.
- **Standard technical terms.** Words such as `abort`, `kill`, and `hang` are established
  system terms; use them where they are the standard name, and prefer neutral words in
  prose.
- **Quotation.** When quoting a source, a log, or someone's words for a legitimate
  reason, reproduce them accurately and note why.
- **Historical records.** Past commits, released changelogs, and archived discussions are
  not rewritten.

Where an exception applies, a short comment explaining it prevents repeated questions.

## Applying This Guide

### Writing

Contributors check their own text against the table. The preferred term is a suggestion
that improves clarity, so a different clear, respectful wording is also fine.

### Review

Reviewers may suggest a change when they see a term from the "Instead of" column, using a
friendly, specific comment that names the preferred term and the reason. A suggestion is
not a block unless the term is on the list of terms to avoid, or appears in text users
will see. Authors are not asked to rewrite unrelated code only to change a term.

### Existing text

Existing occurrences are fixed **opportunistically**: when someone is already editing the
file, or in a small focused change. A repository-wide rename is not required, and is
done only as its own reviewed change so it is easy to verify. For example, a search of
`docs/` finds an occurrence of "whitelist" in `docs/TREE_SHAKING.md`, which would be
corrected the next time that document is edited.

### Enforcement

This guide is enforced by **review and conversation**, not by an automated gate. No lint
rule or CI check enforces it today. If the maintainers later add one, it must use an
allowlist of approved exceptions so that standard technical terms and external names do
not cause false failures, and it would be introduced under the
[automation policy](../policies/BOT_AUTOMATION.md).

A reviewer who is unsure how to raise a concern can ask the
[Community Manager](../roles/COMMUNITY_MANAGER.md). A person who is hurt by language in a
project space can report it through the conduct process without needing to justify it.

## Maintaining This Guide

The Community Manager and the Docs Lead maintain it. Anyone may propose an addition or a
change by opening an issue or a pull request that touches only the `Governance/` folder,
with the reason for the change. The guide is reviewed at least once a year, because
language and community norms change.

## Related Documentation

- [Code of Conduct Enforcement](../COC_ENFORCEMENT.md) and [Reporting](../COC_REPORTING.md)
- [Values](../VALUES.md)
- [Glossary](../GLOSSARY.md)
- [Community Manager role](../roles/COMMUNITY_MANAGER.md) and [Docs Lead role](../roles/DOCS_LEAD.md)
