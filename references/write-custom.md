# Write from a saved profile

The profile resolved in `SKILL.md` is the template for this run. Fill that folder's `template.md`. The section labels, their order, the checkbox lines, and the language come from that file.

Match the voice and density of `example.md` in the same folder when it exists. When the profile has no `example.md`, follow the template's headings and write at the density of a body a reviewer can merge from.

The output is the filled template in one fenced code block, with no preamble. Keep code identifiers, commands, status codes, and file names as they appear in the repo.

Produce the body only. Open or update the pull request on the forge only when the user asks for that.

The default Portuguese rules in `SKILL.md` describe the bundled template. Here, map those rules onto this template.

## Gather

Read enough of the change to name real symbols. The branch name is a hint. The diff is the source.

Use the same seven checks as the default section of `SKILL.md`: project name, branch and base, commits and the behavior hunks, the chat, the task id, the test command, and docs plus branch sync.

Apply them through this profile:

- Write a project-name line when the template has one. Use the name the team calls the service.
- Write a task line when `profile.md` has a `Task line` other than `none`. Use the tracker shape already in the template or the profile (ClickUp, Jira, Linear, or the link they use). Reuse a team id that already appears in the conversation or the repo. When there is no task id, leave their placeholder as written and explain that in the notes heading, in the template's language. Invent no id.
- When `Task line` is `none`, add no ticket line.
- Record the test command the same way as the default: a command and result already in the chat or terminal, then the repo's `package.json` scripts, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Makefile`, `justfile`, or CI workflow.
- Put every fact you could not verify in the `Notes heading` from `profile.md`, in the template's language. When `Notes heading` is `none`, put those sentences in the last prose section of the template.

## Sections

Check every box the diff actually is. Leave the rest empty. More than one box is right when the change really is more than one kind of work.

A breaking-change question in the template gets exactly one answer. Mark the affirmative when existing consumers must change to keep working: response body, a status code clients branch on, an event payload, a public CLI, an env var, or a schema. Name that contract in the notes heading. Mark the negative when the change is internal or compatible.

## How it was tested

Say how this project was tested, with the real command, in the template's language.

- When the suite ran in this chat or the terminal, name the command and the outcome: the kind of test, the command, then the counts you actually saw. Include end-to-end or integration only when those ran.
- When tests exist and nobody ran them, say they were not run in this session and name the command the repo expects. Invent no suites, counts, or passing result.
- When the repo has no automated suite, say what was checked manually, or that nothing was run.

## Checklist

Check a box only with the evidence below. An empty box is the correct output when the evidence is missing. Explain every empty box in the notes heading, in one short sentence, in the template's language.

- Project standards — you read the diff and the commits, and they follow the patterns already in the repo (or the user said they do). Leave it open when the commits contradict the repo's style.
- Self-review — you read the behavior hunks, not only the stat or the commit list. Leave it open when you only skimmed.
- Docs — check when docs were updated, or when you looked and the change needs no doc update. Leave it open when you did not look.
- Tests — check when this bugfix or feature has tests added or updated in the diff. Leave it open when the tests were not added, when you could not see them, or when the change is docs-only.
- Branch updated — check only after comparing the branch to the target tip (`git fetch` and `git rev-list --left-right --count <base>...<branch>`, or the forge's behind count). A clean working tree is a separate fact. Leave it open until that comparison exists.

Map each of those bars onto the checkbox the template actually has. A template that has no checklist gets the same facts in the notes heading, as sentences, and no invented checkbox block.

## Notes heading

Write the context a reviewer needs that does not fit the description: the new contract, status codes, what was left unchanged, a missing task id, the base branch you assumed, and why a checklist box is open. Keep it concrete. Leave the description's paragraphs in the description.
