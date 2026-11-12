---
name: make-a-pr
description: >
  Write a pull request body, open or update the GitHub pull request, or save
  a project's PR template for next time. Use when the user asks for a PR
  description, PR body, pull request text, or invokes make-a-pr. Use when
  the user asks to open or update the pull request on GitHub. Use when the
  user teaches a new project's PR format and wants it saved. Use when
  the user states a lasting correction for the next pull request in
  this project. The bundled Portuguese template is the fallback. A saved
  project profile with its own template wins. Otherwise a pull request
  template already in the repository is the format.
---

# Make a PR

Two jobs, plus a correction. Pick before writing.

**Save a profile** when the user is teaching how pull requests work in a project: they say it is a new project, they send a template file or a format, or they ask to remember that format. Read [references/configure.md](references/configure.md) and finish that job. When they also asked for the body in the same message, save first, then write with the profile you just saved.

**Save a correction** when the user states a lasting rule for this project. Read [references/preferences.md](references/preferences.md). When the message also asks to rewrite this body, save the line and rewrite.

**Write the body** otherwise. Resolve the template, then fill it. Apply every line in the matching folder's `preferences.md`, as [references/preferences.md](references/preferences.md) describes. End every body with the trailer in [references/coauthor.md](references/coauthor.md). Before any publish, follow [references/ask.md](references/ask.md). When the user has already asked to open or update the pull request on GitHub, publish with [references/publish.md](references/publish.md) after the body is ready. When they have already asked for the text only, stop at the fenced body. When they have not chosen, show the body, ask, and stop.

## Resolve the template

Match a saved profile first, then a template the repository already has, then the default.

1. List `references/projects/*/profile.md` beside this skill. Find the skill directory with the same search order as [references/configure.md](references/configure.md).
2. A profile matches when any line under `Match` equals, case-insensitively, the repo folder name, the package name, or the origin repo name. When the user named the project in the chat, that name wins over another match. When two profiles still match, use the one whose folder name equals the repo folder.
3. A folder that contains `template.md` supplies that file, its `example.md` when present, and its `profile.md`. Follow [references/write-custom.md](references/write-custom.md). Leave the default section below unread. When the repository also has a pull request template, say once, before the body, that the saved profile was the one used.
4. A folder with no `template.md` does not replace the format. Continue below. Its `preferences.md` still applies.
5. No `template.md` in the folder: follow [references/repo-template.md](references/repo-template.md). A file there is this run's template. Follow [references/write-custom.md](references/write-custom.md) and leave the default section unread.
6. No `template.md` and no repository template: the default below, plus the suggestion in [references/repo-template.md](references/repo-template.md).

## Default

Write the pull request body by filling [references/template.md](references/template.md). Match the voice and density of [references/example.md](references/example.md). The section labels, their order, and the checkbox lines are fixed. Keep those labels in Portuguese. A saved profile is a separate folder under `references/projects/`. The default files stay as they are. Lines in that project's `preferences.md` apply on top of this template.

The output is the filled template in one fenced code block, with no preamble, and the co-author trailer from [references/coauthor.md](references/coauthor.md) as the last line. Anything you could not verify goes inside **Informações adicionais**, in Portuguese, as part of the body.

Write the body in Portuguese. Keep code identifiers, commands, status codes, and file names as they appear in the repo.

## Gather context before writing

Read enough of the change to name real symbols. Do not fill the template from the branch name alone.

1. Project name. Use the name the team calls the service (repo folder, package name, or the name already used in the chat). The first line of the body is that name, in the same style as `Moderation-Microservice`. It is not a sentence.
2. Branch and base. Current branch, and the target branch (the open PR's base, or the repo default when there is no PR yet). Name the base you assumed in **Informações adicionais** when it is not obvious.
3. Commits on this branch against the base, then the diff. Read the hunks that change behavior. A stat line is not enough for the descrição or for the self-review checkbox.
4. The chat. Decisions, out-of-scope notes, task links, and commands already run. Prefer what the user said and what the diff shows over a guess.
5. Task id. Look in the chat, the branch name, and commit messages for a ClickUp, Jira, or Linear id. For ClickUp, the link looks like `Implementa [#1gaax8j7rwb](https://app.clickup.com/t/36971573/1gaax8j7rwb)`. Reuse a team id that already appears in the conversation or the repo. If there is no task id, leave `Implementa [#TASK_ID](CLICKUP_URL)` and write in **Informações adicionais**: `Não há ID do ClickUp nesta conversa: troca TASK_ID pelo número da task antes de abrir o PR.` Never invent an id.
6. Tests. Find how this repository runs tests. Do not assume npm. Check, in order: a command and result already in the chat or terminal; then `package.json` scripts, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Makefile`, `justfile`, or the CI workflow. Record the command you would actually run here.
7. Docs and branch sync. Docs follow the checklist rule below. The branch comparison runs on every body, as [references/branch-sync.md](references/branch-sync.md) describes.

If the diff is large, group it by behavior and read the files that implement each behavior. Skip lockfiles and generated files unless the PR is about them.

## Descrição

First paragraph: one or two sentences. What changed for the caller or the operator, and the mechanism, with the important type or function names. Same tone as the example (`Padroniza o tratamento de erro do caminho HTTP. O controller deixa de engolir a exceção...`).

Then the task line, alone.

Then one short paragraph per meaningful change, present tense, in the order a reviewer should read them. Name the symbol. Do not paste file lists or commit subjects. Leave out drive-by formatting unless that is the point of the PR.

## Tipo de Mudança

Check every box the diff actually is. Leave the rest empty. More than one box is fine when the change really is more than one kind of work.

- Corrige um Bug — fixes a wrong result or a broken path.
- Implementa Funcionalidade — adds behavior, a contract, or a capability that was not there.
- Atualiza o Readme/Documentações — docs are part of the change. Do not check this only because a comment moved.
- Refatoração de código — same behavior, different structure.
- Atualização de pacote/linguagem — dependency, runtime, or language version.
- Ajuste do estilo/qualidade de código — lint, format, rename, or cleanup with no behavior change.
- Ajuste de build/deploy — CI, image, pipeline, env, or release wiring.
- Outro — only when none of the above fit. Replace `___` with a few words. Otherwise leave `___` as written.

The example checks only **Implementa Funcionalidade**. That sample is one change. It is not a rule that a PR has a single type.

## BREAKING CHANGE

Check **Sim** when existing consumers must change to keep working: response body, status code clients branch on, event payload, public CLI, env var, or schema. Name that contract in **Informações adicionais**.

Check **Não** when the change is internal or compatible. A behavior note in **Informações adicionais** can still be **Não**, as in the example, when callers do not have to change.

Check exactly one of the two.

## Como foi testado?

Say how this project was tested, with the real command.

- If the suite ran in this chat or the terminal, name the command and the outcome in the example's shape: what kind of test, the command, then the counts you actually saw (`28 suites, 232 testes, todos passando.`). Include e2e or integration only when those ran.
- If tests exist but nobody ran them, write `Não executei os testes nesta sessão.` and name the command the repo expects. Do not invent suites, counts, or a passing result.
- If the repo has no automated suite, say what was checked manually, or that nothing was run.

## Checklist

Check a box only with the evidence below. An empty box is the correct output when the evidence is missing. Explain every empty box in **Informações adicionais** in one short sentence, the way the example explains the branch item.

- Padrões do projeto — you read the diff and the commits, and they follow the patterns already in the repo (or the user said they do). Leave it open when the commits contradict the repo's style.
- Autorrevisão — you read the behavior hunks, not only the stat or the commit list. Leave it open when you only skimmed.
- Readme/documentações — check when docs were updated, or when you looked and the change needs no doc update. Leave it open when you did not look, as the example does.
- Testes — check when this bugfix or feature has tests added or updated in the diff. Leave it open when the tests were not added, when you could not see them, or when the change is docs-only and you should say that instead of pretending tests were added.
- Branch atualizada — decided by [references/branch-sync.md](references/branch-sync.md), which runs on every body. Check it only when `behind` is 0. A clean working tree is a separate fact. The count goes in **Informações adicionais** as `A branch está <behind> commits atrás de <base>.`

## Informações adicionais

Write the context a reviewer needs that does not fit the descrição: the new contract, status codes, what was deliberately left unchanged, a missing task id, the base branch you assumed, and why a checklist box is open. Keep it concrete. Do not repeat the descrição paragraph by paragraph.

## Adapt to the project

On this default template, the section labels stay fixed. These change with the repo:

- The test command and how results are reported (Jest suites, pytest, `go test`, Cargo, Maven, a Makefile target).
- Where the task id lives (ClickUp URL, `PROJ-123`, Linear).
- What "breaking" means for that repo's consumers.
- The project name on the first line.

When a fact differs from the Moderation example, follow the repo in front of you. The example is a tone and structure reference, not a source of file names, counts, or task ids.

A project whose pull request format is a different document gets its own profile under `references/projects/`. Teach it with [references/configure.md](references/configure.md). That profile supplies the template for that project. This default stays the template for every repo that has no profile.
