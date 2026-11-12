# The question form

Open decisions are one form, not a paragraph. Skip every decision the user already made in this chat. When none remain, show the body and do not ask.

In Cursor, call AskQuestion once. Title: `Pull request`. Put only the still-open questions in that call. Do not repeat the options in prose. One sentence may sit above the form when a fact would otherwise be invisible, then the form is the last thing in the reply.

Outside Cursor, use the same questions as a short numbered menu. Same options, same order. No extra commentary.

Labels below are for the Portuguese template. A saved profile uses that profile's language for the labels. The ids stay the same.

## Before the body

Several repository templates and the user has not named one. This is the whole form. Stop. Write the body only after they choose.

- id `template`. Prompt: `Qual template deste repositório?` One option per file path.

## After the body

Show the fenced body first. Then the form, with whichever of these are still open. Stop. Do not push, do not call `gh`, and do not write `.github/pull_request_template.md` in this turn.

- id `destino`, when [the publish choice](#already-answered) is missing. Prompt: `O que faço com este texto?`
  - `github` — `Subir no GitHub`
  - `text` — `Só o texto`
- id `padrao`, when [repo-template.md](repo-template.md) found no template and the user has not accepted or refused the base file. One sentence above the form: `Este repositório não tem template de pull request. O corpo segue o padrão base.`
  - `save` — `Gravar em .github/pull_request_template.md`
  - `skip` — `Agora não`

## Already answered

Look through the user messages in this chat, including the one that started the skill. The latest choice wins. A click on the form is a choice. Do not show that question again.

GitHub: they asked to open, update, publish, or upload, or they picked `github`. "Abre o PR", "sobe no GitHub", "atualiza o PR", "open the pull request" count. Publish after the body is ready.

Text only: they asked for the description and said they do not want it on GitHub, or they picked `text`. "Só o texto", "apenas a descrição", "não sobe" count. Stop after the body.

`save` writes `references/template.md` to `.github/pull_request_template.md`. Leave the commit for when the user asks. `skip` writes nothing.

A request that only says "make-a-pr", "escreve o PR", or "qual é o texto" has no `destino` yet.

Asking again in the same chat, after they already answered or after the form is waiting, is a miss.
