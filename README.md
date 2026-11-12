# make-a-pr

Agent skill that writes a pull request body from the current chat, the repository in front of the agent, the branch, the diff, and that project's test setup.

The bundled template is this team's Portuguese pull request body. The skill picks a format in this order: a profile you saved for that project, then a pull request template already in the repository, then the Portuguese template. The Portuguese labels stay fixed for that last case.

The agent writes the body. When you ask it to open or update the pull request, it does that on GitHub with `gh`, under the account already logged in. Every body ends with a `Co-authored-by` line for the application that ran the skill (Cursor or Claude Code), so a squash merge records that co-author even when the editor's own attribution toggle is off.

## When it runs

The agent loads this skill when you ask for a PR description, a PR body, or pull request text, when you invoke `make-a-pr`, or when you teach it a new project's pull request format.

Ask in the chat where the change was made, with the branch checked out. The skill reads real symbols from the diff. A branch name alone is not enough context to fill the template.

Triggers that match the skill description:

- "Write the PR description."
- "Fill the pull request body."
- "make-a-pr"
- `/make-a-pr` in clients that expose skills as slash commands
- "This is a new project. Pull requests here use this format." plus the file or the pasted template
- "Abre o pull request."
- "Atualiza o PR que já está aberto."

## What you get

One fenced code block, with no preamble. On the default template the body is Portuguese. A saved project profile uses the language and the headings of the template you saved for that repo. Code identifiers, commands, status codes, and file names stay as they appear in the repository.

Anything the agent could not verify goes inside **Informações adicionais**, in Portuguese, as part of the body.

The last line of the body is always the co-author trailer, after a blank line. Cursor writes `Co-authored-by: Cursor <cursoragent@cursor.com>`. Claude Code writes `Co-authored-by: Claude <noreply@anthropic.com>`. The host is the application, so a Claude model running inside Cursor still signs Cursor.

## Open or update on GitHub

If you already said what you want, the skill does not ask again. "Abre o PR" or "sobe no GitHub" publishes. "Só o texto" stops after the body.

If a choice is still open, the skill shows the body and then one form. In Cursor that form is the question card. Elsewhere it is a short numbered menu with the same options. A paragraph question is not used.

The card only includes what you have not answered:

- `O que faço com este texto?` — Subir no GitHub, or Só o texto.
- When the repository has no pull request template, `Gravar em .github/pull_request_template.md`, or Agora não.
- When the repository has several templates, which file, and that card comes before the body.

Your click or your next message is the choice. The skill uses `gh` on the account already logged in only after you pick GitHub.

- A pull request for the current branch is updated in place (`gh pr edit`), title and body.
- No pull request yet: the branch is pushed with `git push -u origin HEAD`, then opened with `gh pr create`.

The URL comes back in the chat. The skill does not merge, force-push, or change git config. Commits already on the branch stay as they are. A commit the agent creates during the same request carries the same `Co-authored-by` line. On a squash merge, GitHub uses the pull request body as the commit message, so the trailer at the end of the body is the co-author that lands on the default branch.

```text
Nome-Do-Projeto
Descrição
<what changed for the caller or the operator, with the important symbols>

Implementa [#TASK_ID](CLICKUP_URL)

<one short paragraph per meaningful change>

Tipo de Mudança
- [ ] Corrige um Bug
- [x] Implementa Funcionalidade
...
Meu código introduz um BREAKING CHANGE?
- [ ] Sim
- [x] Não
Como foi testado?
<the command this repository actually uses, and the result if it ran>

Checklist
...
Informações adicionais
<contract notes, assumed base branch, missing task id, and why any checklist box is open>
```

[references/example.md](references/example.md) shows the voice and the density. The names, counts, and task id in that file belong to that sample change. The agent takes those facts from the repository in front of it.

## Install

This repository is the skill. `SKILL.md` sits at the root, and the folder you install must be named `make-a-pr`, matching the `name` field in the frontmatter.

Cursor, Claude Code, and Codex all read the [Agent Skills](https://agentskills.io) layout. Cursor's current discovery paths are documented in [Agent Skills](https://cursor.com/docs/skills).

| Scope | Cursor | Also discovered by Cursor |
| --- | --- | --- |
| This machine, every project | `~/.cursor/skills/make-a-pr/` | `~/.agents/skills/make-a-pr/`, `~/.claude/skills/make-a-pr/`, `~/.codex/skills/make-a-pr/` |
| One repository | `<repo>/.cursor/skills/make-a-pr/` | `<repo>/.agents/skills/make-a-pr/`, `<repo>/.claude/skills/make-a-pr/`, `<repo>/.codex/skills/make-a-pr/` |

Personal skills stay on this machine until you sync them for Cloud Agents or publish them with the project. A project install travels with the repository.

Clone straight into a skills directory:

```bash
git clone https://github.com/PietroDub/make-a-pr.git ~/.cursor/skills/make-a-pr
```

Or symlink a checkout you already have:

```bash
mkdir -p ~/.cursor/skills
ln -s /absolute/path/to/make-a-pr ~/.cursor/skills/make-a-pr
```

For a single repository, use the same clone or symlink under `<repo>/.cursor/skills/make-a-pr`.

Restart the agent session after installing so the client picks up the new skill. In Cursor you can also attach the folder with `@` when you want it in that turn.

## Layout

```text
make-a-pr/
├── SKILL.md                      # which job to run, and the default template rules
├── README.md                     # this file
└── references/
    ├── template.md               # default empty template; labels and order are fixed
    ├── example.md                # default filled sample; voice and density only
    ├── configure.md              # how a new project's format is saved
    ├── write-custom.md           # how a saved profile or a repo template is filled
    ├── repo-template.md          # find the repository's own template, or suggest the base
    ├── coauthor.md               # the Co-authored-by line appended to every body
    ├── ask.md                    # GitHub or text only, skipped when already answered
    ├── branch-sync.md            # compare the branch to the base on every body
    ├── preferences.md            # lasting corrections for one project
    ├── tests.md                  # which summary line holds the count for each runner
    ├── publish.md                # open a GitHub PR, or update the one on this branch
    └── projects/
        └── <project-slug>/       # created when you teach a project
            ├── template.md
            ├── example.md        # only when you also sent a filled example
            ├── profile.md        # match names, language, task line, notes heading
            └── preferences.md    # lasting corrections; the template files stay as they are
```

`SKILL.md` is the source of truth for the agent's behavior. This README is the human guide. When the two disagree, follow `SKILL.md` and update this file.

## Template

[references/template.md](references/template.md) is the body. The agent fills it. It keeps the labels in Portuguese and in this order.

| Section | What belongs there |
| --- | --- |
| Project name | First line. The name the team calls the service (`Moderation-Microservice` style). A name, not a sentence. |
| Descrição | One or two sentences on what changed for the caller or the operator, and the mechanism, naming the important types or functions. Then the task line, alone. Then one short present-tense paragraph per meaningful change, in the order a reviewer should read them. |
| Tipo de Mudança | Every box the diff actually is. More than one box is correct when the change really is more than one kind of work. |
| BREAKING CHANGE | Exactly one of **Sim** or **Não**. **Sim** when existing consumers must change to keep working. |
| Como foi testado? | The command this project uses, and the outcome when the suite ran in the chat or the terminal. |
| Checklist | A checked box needs the evidence in the next section. An empty box is the correct output when that evidence is missing. |
| Informações adicionais | The contract, status codes, what stayed unchanged, a missing task id, the base branch when it is not obvious, and one short sentence for every open checklist box. |

### Tipo de Mudança

| Box | Check it when |
| --- | --- |
| Corrige um Bug | The diff fixes a wrong result or a broken path. |
| Implementa Funcionalidade | The diff adds behavior, a contract, or a capability that was not there. |
| Atualiza o Readme/Documentações | Docs are part of the change. |
| Refatoração de código | Same behavior, different structure. |
| Atualização de pacote/linguagem | Dependency, runtime, or language version. |
| Ajuste do estilo/qualidade de código | Lint, format, rename, or cleanup with the same behavior. |
| Ajuste de build/deploy | CI, image, pipeline, env, or release wiring. |
| Outro | None of the above fit. The agent replaces `___` with a few words. Otherwise `___` stays as written. |

### BREAKING CHANGE

**Sim** when an existing consumer must change to keep working: response body, a status code clients branch on, an event payload, a public CLI, an env var, or a schema. The agent names that contract in **Informações adicionais**.

**Não** when the change is internal or compatible. A behavior note in **Informações adicionais** can still sit next to **Não** when callers do not have to change.

## What the agent gathers

Before writing, the agent reads enough of the change to name real symbols.

1. **Project name.** Repo folder, package name, or the name already used in the chat.
2. **Branch and base.** The current branch, and the target branch: the open pull request's base, or the repository default when there is no pull request yet. When the base is an assumption, it is named in **Informações adicionais**.
3. **Commits and diff.** Commits on this branch against the base, then the hunks that change behavior. A stat line is not enough for the descrição or for the self-review checkbox. Large diffs are grouped by behavior. Lockfiles and generated files are skipped unless the pull request is about them.
4. **The chat.** Decisions, out-of-scope notes, task links, and commands already run. What you said and what the diff shows come first.
5. **Task id.** ClickUp, Jira, or Linear, looked up in the chat, the branch name, and the commit messages. A ClickUp line looks like `Implementa [#1gaax8j7rwb](https://app.clickup.com/t/36971573/1gaax8j7rwb)`. The team id is reused when it already appears in the conversation or the repo. With no task id, the line stays `Implementa [#TASK_ID](CLICKUP_URL)` and **Informações adicionais** says: `Não há ID do ClickUp nesta conversa: troca TASK_ID pelo número da task antes de abrir o PR.`
6. **Tests.** The command comes from the chat or the terminal when a run is already there, otherwise from `package.json` scripts, `pyproject.toml`, `go.mod`, `Cargo.toml`, `composer.json`, `Makefile`, `justfile`, or the CI workflow. A count is copied from that run's summary line. The skill does not start a suite to fill this paragraph.
7. **Docs and branch sync.** Required for two checklist items. Rules below.

### How testing is reported

| Situation | What the body says |
| --- | --- |
| The suite ran in this chat or the terminal | The kind of test, the command, then the counts that were actually printed (`28 suites, 232 testes, todos passando.`). End-to-end or integration appears only when those ran. |
| Tests exist and nobody ran them | `Não executei os testes nesta sessão.` plus the command the repo expects. |
| The repo has no automated suite | What was checked manually, or that nothing was run. |

Suite names, counts, and a passing result appear only when that run happened. Jest prints `Test Suites:` and `Tests:`. Vitest prints `Test Files` and `Tests`. pytest prints `N passed` on the last line. Cargo prints `test result:`. Maven prints `Tests run:`. PHPUnit prints `OK (N tests)`. `go test` prints `ok` or `FAIL` per package and has no test total, so the sentence names the packages.

### Checklist evidence

| Box | Checked when |
| --- | --- |
| Padrões do projeto | The agent read the diff and the commits, and they follow the patterns already in the repo (or you said they do). Left open when the commits contradict the repo's style. |
| Autorrevisão | The agent read the behavior hunks, not only the stat or the commit list. Left open when the diff was only skimmed. |
| Readme/documentações | Docs were updated, or the agent looked and the change needs no doc update. Left open when nobody looked. |
| Testes | This bugfix or feature has tests added or updated in the diff. Left open when tests were not added, when the agent could not see them, or when the change is docs-only. |
| Branch atualizada | After `git fetch` and `git rev-list --left-right --count origin/<base>...HEAD` on every body. Checked only when the first number, commits behind the base, is 0. **Informações adicionais** always states that number. |

Every open box is explained in **Informações adicionais** in one short sentence.

## What changes between projects

On the default template, the labels stay fixed. These still change with the repo:

- The test command and how results are reported (Jest suites, pytest, `go test`, Cargo, Maven, a Makefile target).
- Where the task id lives (ClickUp URL, `PROJ-123`, Linear).
- What "breaking" means for that repo's consumers.
- The project name on the first line.

## Template the repository already has

A saved profile that contains `template.md` still wins over the repository file. A folder that only has `preferences.md` does not. With no `template.md`, the skill looks for the file GitHub itself would use. Names are not case sensitive. It reads the default branch, because that is the branch GitHub uses, and the working tree when that branch is not available locally.

The first non-empty file wins:

1. `.github/pull_request_template.md` or `.txt`
2. `pull_request_template.md` or `.txt` in the repository root
3. `docs/pull_request_template.md` or `.txt`

If none of those exist, it looks in `PULL_REQUEST_TEMPLATE/` under `.github/`, the root, and `docs/`. One file there is the format. Several files: the skill asks which one, and does not fall through to the Portuguese template. It also checks the organization's `.github` repository when the project itself has nothing.

The body then follows that file's headings and language. The chat names the path before the body. `references/template.md` stays as it is.

## When the repository has no template

No template means every lookup above was missing or empty, and no saved profile matches. The skill recognizes that by the search, not by the branch name.

The body uses the Portuguese base. After it, the chat suggests that base:

`Este repositório não tem template de pull request. O corpo segue o padrão base.`

That sentence sits above the question card, which offers to write the file or to skip. It stays out of the pull request body. The question does not appear again after you accept or refuse. On a yes, the skill writes `references/template.md` to `.github/pull_request_template.md` and leaves the commit for when you ask.

## Corrections for one project

After a body, a lasting rule is stored in `references/projects/<projeto>/preferences.md`. One sentence per line. The Portuguese template and a repository `pull_request_template` stay untouched.

"Daqui pra frente a descrição fica em um parágrafo" rewrites this body and appends that line. "Encurta a descrição" rewrites this body only. A task id, a test count, and a fact that is true only of this diff are not stored.

A project that had no folder gets `profile.md` plus `preferences.md`, and no `template.md`. The next body still uses the repository template or the Portuguese base, with those lines applied on top. The chat names the path and the line, outside the pull request. Saying to forget a line deletes that line.

## Another project's format

Teach the skill while you have that project open. Send the template file, or paste the format, and say that this project uses it.

```text
This is a new project. Pull requests here use this format.
```

The agent saves a profile under `references/projects/<repo-folder>/` inside the skill install (`~/.cursor/skills/make-a-pr` when that is where the skill lives). It stores:

- `template.md` — the format you sent, with your labels and language
- `example.md` — only if you also sent a filled pull request to copy the voice from
- `profile.md` — the names that select this profile (repo folder, package name, origin repo name), the language, the task-line placeholder, and the heading where unverified notes go

`references/template.md` and `references/example.md` stay the bundled base. The next time you ask for a pull request in a repo whose folder, package name, or origin name is on that profile's `Match` list, the body follows the saved `template.md`, even when the repo also has a `pull_request_template` file. A repo with no `template.md` and no template file gets the Portuguese base. Lines in `preferences.md` apply on top of whichever format was chosen.

Send a new file for the same project to replace its template. Say to forget the project and the agent removes that folder, so the repo returns to the default. The skill writes the files and leaves the commit for when you ask.

## Sample

[references/example.md](references/example.md) is a filled body for one change in a service named Moderation: HTTP error handling, with **Implementa Funcionalidade** checked, breaking change set to **Não**, and the README and branch-sync boxes left open on purpose. That sample is one change. A pull request can check more than one type when the diff is more than one kind of work.

The opening of that sample:

```text
Moderation-Microservice
Descrição
Padroniza o tratamento de erro do caminho HTTP. O controller deixa de engolir a exceção, o service lança um AppException com código e status, e o Nest responde com esse status.

Implementa [#1gaax8j7rwb](https://app.clickup.com/t/36971573/1gaax8j7rwb)
```

Copy the shape and the voice from the example. Take the file names, counts, and task id from the change you are describing.
