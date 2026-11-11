# Ask before publishing

Check this at the start of a write, before any `gh` or `git push`. The question is the gate on publishing. Skip it when the user already chose.

## Already answered

Look through the user messages in this chat, including the one that started the skill. The latest choice wins.

GitHub, when they asked to open, update, publish, or upload the pull request. "Abre o PR", "sobe no GitHub", "atualiza o PR", "open the pull request" are a choice. Publish after the body is ready. Do not ask.

Text only, when they asked for the description, the body, or the text and said they do not want it on GitHub. "Só o texto", "apenas a descrição", "não sobe" are a choice. Show the fenced body and stop. Do not ask.

A request that only says "make-a-pr", "escreve o PR", or "qual é o texto" has no choice yet.

## Not answered yet

Write the body and show the fenced block. Then ask, in the language of the template, and stop. Do not push and do not call `gh` in this turn.

Default template, this wording:

"Quer que eu suba esse PR no GitHub, ou prefere só o texto?"

A saved profile uses the same question in that profile's language. Two answers, nothing else: upload to GitHub, or the text only.

When a structured question tool is available, use it with those two options. Otherwise ask in the message. The question is the last thing in the reply.

On the next message, their answer is the choice. Follow it and do not ask again. Asking a second time in the same chat, after they already answered or after you already asked and they have not replied, is a miss.
