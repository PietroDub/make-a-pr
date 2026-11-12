# Project corrections

A correction is a lasting rule for this project. It lives in `preferences.md` inside that project's folder. `references/template.md` and `references/example.md` stay as they are. A repository `pull_request_template` stays as it is.

The folder is the one from [configure.md](configure.md), under `references/projects/<slug>/`. Create the folder when it is missing. When it has no `profile.md`, write one with the project name and the `Match` lines from that file: repo folder, package name, origin repo name. Do not create `template.md` for a correction.

## What gets a line

Append one plain sentence, in the template's language, when the user states a rule for the next pull request. "Daqui pra frente", "sempre", "o time não cita", and "o nome é" are that kind of message.

A message that only fixes this pull request rewrites this body and writes no line. "Encurta a descrição" and "este PR não mexe no webhook" are that kind of message.

A message that does both rewrites this body and appends the line.

Leave these out of `preferences.md`: a task id, a test count, a passing result, a checkbox mark, and a fact that is true only of this diff.

One rule per line. Skip a line that repeats a rule already in the file.

```text
A descrição fica em um parágrafo.
Não citar pending_review no corpo.
A primeira linha é Moderation-Microservice.
```

## After it is saved

Tell the user, outside the pull request body, the path and the line:

`Guardei em references/projects/<slug>/preferences.md: <linha>`

When they refuse that line in this chat, delete it. When they name a line to forget, delete that line and leave the others. Delete `preferences.md` when no line remains. Leave `profile.md` in place.

A correction turn does not ask the publish question again when this chat already chose. It does not commit unless the user asks.

## On the next body

Read `preferences.md` whenever the matching folder has one. Apply every line on top of the template this run already chose: the profile's `template.md` when that file exists, otherwise the repository template, otherwise the Portuguese default.

A line yields when it would hide the change this pull request is about. Say so in the chat, outside the body, and write the body without that line.
