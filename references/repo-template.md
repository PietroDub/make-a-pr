# Repository pull request template

Use this only when no saved profile matches. A profile under `references/projects/` is the format for that project. This file never overrides one.

GitHub fills a pull request from a template on the repository's default branch. Filenames are not case sensitive. Read the default branch, then the working tree if the default branch is not available locally.

## Where a template lives

The single file GitHub shows by default is `pull_request_template.md` or `pull_request_template.txt` in one of these places. Take the first that exists and is not empty:

1. `.github/`
2. the repository root
3. `docs/`

Multiple templates live in a `PULL_REQUEST_TEMPLATE` directory in those same three places, `.md` or `.txt`. Look there only when no single file above exists.

- The user named a file: use that file.
- One file in those directories: use it.
- More than one: list the paths and ask which to fill. Stop. Leave the Portuguese template unused until they choose.

An organization's default community-health file counts only when the repository itself has none. Try `gh api repos/<owner>/.github/contents/pull_request_template.md` and `contents/.github/pull_request_template.md`. A 404 or an empty body means there is no template. Do not block the body on an API error.

## A file was found

That file is the template for this run. Follow [write-custom.md](write-custom.md). There is no `example.md`. Match the file's headings, order, checkboxes, and language.

Before the fenced body, one sentence naming the path, for example: `Formato deste repositório: .github/pull_request_template.md.`

Leave `references/template.md` unchanged. Do not copy the file into `references/projects/` unless the user asks to save a profile.

## Nothing was found

The repository has no pull request standard when every lookup above is missing or empty, and no saved profile matches.

Fill [template.md](template.md) and [example.md](example.md) as the default section of `SKILL.md` describes. In the chat, after the fenced body, suggest the base in Portuguese:

`Este repositório não tem template de pull request. Procurei pull_request_template em .github/, na raiz e em docs/, a pasta PULL_REQUEST_TEMPLATE, e o .github da organização. O corpo segue o padrão base. Quer que eu grave esse padrão em .github/pull_request_template.md?`

Skip that sentence when the user already accepted or refused it in this chat.

On an acceptance, write `references/template.md` to `.github/pull_request_template.md` in the repository. Leave the commit for when the user asks. Do not put this suggestion inside the pull request body.
