---
name: make-a-pr
description: >
  Fill the team's Portuguese pull request template from the current chat,
  repository, branch, diff, and test setup. Use when the user asks for a PR
  description, PR body, pull request text, or invokes make-a-pr. Each project
  keeps its own test command and task tracker; the template sections stay the same.
---

# Make a PR

Write the pull request body by filling [references/template.md](references/template.md). Match the voice and density of [references/example.md](references/example.md). The section labels, their order, and the checkbox lines are fixed. Do not add sections, translate the labels, or swap in another team's template.

The output is the filled template in one fenced code block, with no preamble. Anything you could not verify goes inside **Informações adicionais**, in Portuguese, as part of the body.

Write the body in Portuguese. Keep code identifiers, commands, status codes, and file names as they appear in the repo.

Produce the body only. Open or update the pull request on the forge only when the user asks for that.

## Gather context before writing

Read enough of the change to name real symbols. Do not fill the template from the branch name alone.

1. Project name. Use the name the team calls the service (repo folder, package name, or the name already used in the chat). The first line of the body is that name, in the same style as `Moderation-Microservice`. It is not a sentence.
2. Branch and base. Current branch, and the target branch (the open PR's base, or the repo default when there is no PR yet). Name the base you assumed in **Informações adicionais** when it is not obvious.
3. Commits on this branch against the base, then the diff. Read the hunks that change behavior. A stat line is not enough for the descrição or for the self-review checkbox.
4. The chat. Decisions, out-of-scope notes, task links, and commands already run. Prefer what the user said and what the diff shows over a guess.
5. Task id. Look in the chat, the branch name, and commit messages for a ClickUp, Jira, or Linear id. For ClickUp, the link looks like `Implementa [#1gaax8j7rwb](https://app.clickup.com/t/36971573/1gaax8j7rwb)`. Reuse a team id that already appears in the conversation or the repo. If there is no task id, leave `Implementa [#TASK_ID](CLICKUP_URL)` and write in **Informações adicionais**: `Não há ID do ClickUp nesta conversa: troca TASK_ID pelo número da task antes de abrir o PR.` Never invent an id.
6. Tests. Find how this repository runs tests. Do not assume npm. Check, in order: a command and result already in the chat or terminal; then `package.json` scripts, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Makefile`, `justfile`, or the CI workflow. Record the command you would actually run here.
7. Docs and branch sync, because two checklist items depend on them. See the checklist rules below.

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
- Branch atualizada — check only after comparing the branch to the target tip (`git fetch` and `git rev-list --left-right --count <base>...<branch>`, or the forge's behind count). A clean working tree is not the same thing. The example leaves this open on purpose. Do not check it from memory.

## Informações adicionais

Write the context a reviewer needs that does not fit the descrição: the new contract, status codes, what was deliberately left unchanged, a missing task id, the base branch you assumed, and why a checklist box is open. Keep it concrete. Do not repeat the descrição paragraph by paragraph.

## Adapt to the project

The template does not change between repos. These do:

- The test command and how results are reported (Jest suites, pytest, `go test`, Cargo, Maven, a Makefile target).
- Where the task id lives (ClickUp URL, `PROJ-123`, Linear).
- What "breaking" means for that repo's consumers.
- The project name on the first line.

When a fact differs from the Moderation example, follow the repo in front of you. The example is a tone and structure reference, not a source of file names, counts, or task ids.
